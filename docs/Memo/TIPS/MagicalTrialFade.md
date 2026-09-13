# 魔法少女ノ魔女裁判 Switch2のフェードを再現してみる
最近まのさばを少しずつプレイしてたけど、1ヵ月くらいかけて何とか終わりまで来た.  
久々のADVということで慣れないところがありつつも、なんとか完走！  
値段もお手頃なので、こんなに楽しめて良いのですか！？という気分になれたので、皆にもやってほしいなと思っております.  

という前置きは置いておいて、まのさばをやっているときFadeがちょっと変わった形式で、これくらいなら再現も簡単じゃないかな？とふと思ったのがこれを書き始めた動機です.  
普通のフェードと言えばだんだん暗くなる的なのが多いけど、このゲームでのフェードは基本的に左から右に冊状に区分をしつつ、速度を変えて暗くしていく方式.  
これならSiv3Dでも再現が簡単にできるため、実際にコードを書いて検証をしてみる.  

まず最初はshader側から.  
今回はcbuffer側でパラメータでまとめてパラメータを用意することにした.  
```c++
cbuffer Resource : register(b1)
{
	float g_modScale;
	float g_fadeScale;
	float g_gapScale;
	bool g_isFadeIn;
}
```
これを使って上手く冊状にフェードを行えばよい.  

実際の処理はPixel Shaderに記述していく.  
Vertex Shaderに関しては特に書かずにSiv3D側にあるもので任せる.  
理由は単純で特に頂点毎に処理したいものはないからである.  
要はポストプロセス的要領で掛けていくことになるわけだね.  
```c++
float4 PS(s3d::PSInput input) : SV_TARGET
{
    // ...
}
```

まず最初はTexture Sampling,現状の描画物を拾って来るだけ.  
```c++
	float2 uv = input.uv;
	float4 texColor = g_texture0.Sample(g_sampler0, uv);
```

そしたら次に黒の領域を計算.  
これはuvの位置に対してmodで冊状に分離.  
このmod処理だけであれば全部の冊が同じ速度でfadeすることになるが、今回は冊毎に速度の変更が必要.  
ということで何をするかというと、uvに対してscaleを掛けてあげて速度差をつけてあげる.  
例えば0.25でmodをしてるとして、`mod(0.1,0.25) = 0.1`, `mod(0.35, 0.25)=0.1`となる.  
けど、uvだけで見れば前者は0.1,後者は0.35という差がついてるので、これを利用して速度を変えてあげようという目論見.  
```c++
	// make black region
	float modifiedU = fmod(uv.x, g_modScale) + uv.x * g_gapScale;
```

後はLiner Interplateで上手くFadeをしてあげるだけ.  
黒になるかどうかはまのさばの場合ぱっきり別れてそうなので、step関数で処理.  
また`g_isFadeIn`で反転、何しているかというとフェードイン/フェードアウトの切り替えができますよくらいのもの.  
最後にLerpしてあげれば終了.これでshader側は終わり.  
```c++
	float scale = step(modifiedU, g_fadeScale);
	scale = g_isFadeIn ? 1.0f - scale : scale;
    texColor.rgb = lerp(texColor.rgb, 0.0f, scale);
    return (texColor * input.color) + g_colorAdd;
```

次はC++側を見ていく.  
まずはShader側の用意から.  
struct定義をしておき、Shader自体の用意とConstant Bufferを用意する.  
```c++
	struct ShaderParam
	{
		float m_mod = 0.1f;
		float m_scale = 0.0f;
		float m_gapScale = 0.1f;
		bool m_isFadeIn = false;
	};

	const PixelShader pixelShader = HLSL{ U"example/shader/hlsl/magical_trial_fade.hlsl", U"PS" };
	ConstantBuffer<ShaderParam> cb;
```

また、今回はテストでフェードイン/フェードアウトをしたい.  
そのためFade用のパラメータとタイマーを用意.  
```c++
	enum class Fade { In, Out };
	Fade fadeState = Fade::Out;
	bool isAnim = false;
	Timer timer{ 1.0s };
```

PixelShaderはRenderTextureの描画時に行うようにすればOK!  
```c++
// Pixel Shader
{
    Graphics2D::SetPSConstantBuffer(1, cb);
    const ScopedCustomShader2D shader{ pixelShader };
    renderTexture.draw();
}
```

今回のパラメータの設定はsliderでmodとgapを設定できるようにする.  
SimpleGUIってdoubleしか渡せないのでちょっと冗長な書き方に...  
もう少しシンプルにfloat渡せる方法あれば誰か教えてください...  
アニメーションの開始はボタンで制御.  
押したらフェードイン、フェードアウトが発生します.  
```c++
double v = cb->m_mod;
SimpleGUI::Slider(U"mod", v, 0.01, 1.0, Vec2{ 10, 10 }); cb->m_mod = static_cast<float>(v);
v = cb->m_gapScale;
SimpleGUI::Slider(U"gap", v, 0.001, 0.1, Vec2{ 10, 90 }); cb->m_gapScale = static_cast<float>(v);

if (SimpleGUI::Button(U"Fade", Vec2{10,140}))
{
    if (!timer.isRunning())
    {
        timer.restart();
        isAnim = true;
    }
}
```

タイマーが発生すると以下のような処理が走る.  
基本的にタイマーが作動中は`m_scale`を0~0.4に変換.  
アニメーションが終わるとFadeInならFadeOutに,FadeOutならFadeInになるように移行して、アニメーションを止めておく.  
```c++
if (isAnim)
{
    if (timer.reachedZero())
    {
        isAnim = false;
        switch (fadeState)
        {
        case Fade::In:
            fadeState = Fade::Out;
            cb->m_isFadeIn = false;
            break;

        case Fade::Out:
            fadeState = Fade::In;
            cb->m_isFadeIn = true;
            break;
        }

        cb->m_scale = 0.0f;
    }
    else
    {
        cb->m_scale = timer.progress0_1() * 0.4;
    }
}
```

これで終わったので結果を見てみよう.  

<blockquote class="bluesky-embed" data-bluesky-uri="at://did:plc:dymg72hugpzndmgyqhyhgx2i/app.bsky.feed.post/3mveoefho7c2a" data-bluesky-cid="bafyreifg7nrjp2iydermt25axj7mbeaktrtlqy3y2ql2h5axgc3lfr5tpi" data-bluesky-embed-color-mode="system"><p lang="ja">まのさばを最後までやったので、まのさばで印象に残ってるFadeをそれっぽく遊びで作ってみたやつ
雑なpixel shaderを生産しました<br><br><a href="https://bsky.app/profile/did:plc:dymg72hugpzndmgyqhyhgx2i/post/3mveoefho7c2a?ref_src=embed">[image or embed]</a></p>&mdash; tera (<a href="https://bsky.app/profile/did:plc:dymg72hugpzndmgyqhyhgx2i?ref_src=embed">@terakoya-cafe-terrace.net</a>) <a href="https://bsky.app/profile/did:plc:dymg72hugpzndmgyqhyhgx2i/post/3mveoefho7c2a?ref_src=embed">2026年9月13日 12:40</a></blockquote><script async src="https://embed.bsky.app/static/embed.js" charset="utf-8"></script>

今回は通常の描画に何もないのは寂しいので、ボールが動き回るような処理を書いてみた.  
動き回りつつ、他のボールに当たったら生成消滅をしつつ逆方向に跳ね返る...まあ今回の本題には関係ない内容です.  
いい感じにフェードできているかとは思う、実際はパラメータ調整が必要だけど、自分はそれっぽい再現が出来ればよかったので今回の目標は達成！  

今回の実装を行うにあたって他の方の実況でSteam/Switch版も確認してたんだけど、フェードがなくてあれ～...となっていた.  
んでSwitch2版をプレイしているので確認したらある...勘違いかもしれないけど、もしかしたらこのフェードはSwitch2版から追加されたものなのかもしれない.  
もしかしたらパッチ当たって実は最新だとSteam/Switch版でも出るのかもだけど、自分はSwitch2しかやってないのでそれ以上は知らないマン.  
こういうちょっとした見た目が変わるもの、コストは低いけど見た目が変わるという点でプレイヤーとしてはちょっと嬉しいポイントではあったりするよね.  
実際に作る際は汎用性持たせるのであれば、アーティストにも弄ってもらうことを考えてルール画像による実装当りがよさそうとは思った.  
ルール画像なら他のパターンも作りやすいしね、場面によってフェード切り替え演出とかもできるし.  
けど、時間がない状態でアーティストもいないならshaderで頑張ればこの辺は簡単に行けるし、アーティストでもshader分かればこれくらいは簡単にできると思う.  

といったところで今回はこの辺で締めておこうかな.  