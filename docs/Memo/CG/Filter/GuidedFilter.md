# Guided Filter  
以前にBilateral Filterの実装[^1]を行ったが、これのちょっと拡張提案にGuided Filterというものが存在する.  
今回はこれに迫っていくことにしよう、元の論文はこれ[^2].  
説明としてはEnglishの方のWiki[^3]が分かりやすい、というか論文の解説部分の簡略説明として優秀過ぎないか？  

Guided Filterでは以下のような線形モデルを仮定する.  

```math
q_{i} = a_{k}I_{i}+b_{k}
```
ここで、

* $`a_{k},b_{k}`$: フィルタ内の線形関数  
* $`I_{i}`$: ガイド画像
* $`q_{i}`$: 出力画像  

となる.  
フィルタウィンドウは一番有名な半径$`r`$のボックスフィルタと仮定.  

そして、入力画像を$`p_{i}`$とする際、この$`p_{i},q_{i}`$が同じような値になるような制約を与えることで$`a_{k},b_{k}`$を決定する.  

ここで、制約を$`E(a_{k},b_{k})`$とすると、

```math
\begin{split}
E(a_{k},b_{k}) 
&= \sum_{i} (q_{i}-p_{i})^2 + \epsilon a_{k}^{2} \\
&= \sum_{i} (a_{k}I_{i} + b_{k} - p_{i})^2 + \epsilon a_{k}^{2}
\end{split}
```

となる.  
$`\epsilon`$は正則化パラメータで$`a_{k}`$が大きくなりすぎないように補正をしているだけ.  
これを線形回帰で求めると以下の形になる.  

```math
\begin{split}
& a_{k} = \frac{\frac{1}{|\omega|} \sum_{i} I_{i}p_{i} - \mu_{k} \bar{p}_{k}}{\sigma_{k}^{2} + \epsilon} \\
& b_{k} = \bar{p}_{k} - a_{k}\mu_{k}
\end{split}
```

パラメータが多すぎる...一つずつ見ていこう.  

* $`\mu_{k}`$: $`I_{i}`$の平均
* $`\sigma_{k}^{2}`$: $`I_{i}`$の分散  
* $`\bar{p}_{k}`$: $`p_{i}`$の平均

要は画像に対して平均と分散を上手く使えばよいわけだ.  
あとはこれを線形モデルに当てはめるだけなんだけど、$`q_{i}`$自体ウィンドウの値によって変わってしまうため平均化することで均してやる.  

```math
\begin{split}
q_{i} &=
\frac{1}{|\omega|} \sum_{k}(a_{k}I_{i}+b_{k}) \\
&= \bar{a}_{i}I_{i} + \bar{b}_{i}
\end{split}
```

こんな感じで平均化した$`\bar{a}_{i},\bar{b}_{i}`$を使うようにすればよい.  
ただし、$`\bar{a}_{i} = \frac{1}{|\omega|} \sum_{k}a_{k}`$であり、$`\bar{b}_{i} = \frac{1}{|\omega|} \sum_{k}b_{k}`$である.  

さて、ここまで分かれば後は実際にコードを組むだけ.  
今回はまず平均値を計算するのに以下の3つの関数を用意した.  

```c++
auto CalculateMean = [&](const int w, const int h, const Array<Array<double>>& data)
    {
        double mean = 0.0;
        for (int fw = -FILTER_SIZE; fw <= FILTER_SIZE; fw++)
        {
            for (int fh = -FILTER_SIZE; fh <= FILTER_SIZE; fh++)
            {
                int cw = Clamp(w + fw, 0, WIDTH - 1);
                int ch = Clamp(h + fh, 0, HEIGHT - 1);

                mean += data[ch][cw];
            }
        }

        return mean / FILTER_NUM;
    };

auto CalculateRMS = [&](const int w, const int h, const Array<Array<double>>& data)
    {
        double mean = 0.0;
        for (int fw = -FILTER_SIZE; fw <= FILTER_SIZE; fw++)
        {
            for (int fh = -FILTER_SIZE; fh <= FILTER_SIZE; fh++)
            {
                int cw = Clamp(w + fw, 0, WIDTH - 1);
                int ch = Clamp(h + fh, 0, HEIGHT - 1);

                mean += data[ch][cw] * data[ch][cw];
            }
        }

        return mean / FILTER_NUM;
    };

auto CalculateMul = [&](const int w, const int h, const Array<Array<double>>& a, const Array<Array<double>>& b)
    {
        double mean = 0.0;
        for (int fw = -FILTER_SIZE; fw <= FILTER_SIZE; fw++)
        {
            for (int fh = -FILTER_SIZE; fh <= FILTER_SIZE; fh++)
            {
                int cw = Clamp(w + fw, 0, WIDTH - 1);
                int ch = Clamp(h + fh, 0, HEIGHT - 1);

                mean += a[ch][cw] * b[ch][cw];
            }
        }

        return mean / FILTER_NUM;
    };
```
対応としては以下の感じ.  

* `CalculateMean`: 平均計算用  
* `CalculateRMS`: 二乗平均計算用
* `CalculateMul`: 2つの値の積の平均計算用  

これらを駆使して計算をしていく.  

まずはaとbの計算.愚直に計算をしていく.  

```c++
for (int w = 0; w < WIDTH; w++)
{
    for (int h = 0; h < HEIGHT; h++)
    {
        double meanI = CalculateMean(w, h, I);
        double meanP = CalculateMean(w, h, p);

        double var = CalculateRMS(w, h, I) - meanI * meanI;
        double cov = CalculateMul(w, h, I, p) - meanI * meanP;

        double a_denominator = cov;
        double a_numerator = var + EPSILON;

        a[h][w] = a_denominator / a_numerator;
        b[h][w] = meanP - a[h][w] * meanI;
    }
}
```
このあとはaとbの平均値を求めれば、qに必要なものは揃ったことになる.  
なので、平均値計算後にqを計算すれば終わり.  
```c++
for (int w = 0; w < WIDTH; w++)
{
    for (int h = 0; h < HEIGHT; h++)
    {
        double aMean = CalculateMean(w, h, a);
        double bMean = CalculateMean(w, h, b);

        double q = aMean * I[h][w] + bMean;

        result[h][w] = ColorF(Vec3::One() * q, 1.0);
    }
}
```

今回の計算はグレースケールに限定しているが、カラー画像の場合も3Dに対して同じ処理をやればいいだけの話.  

さて、そしたら結果を見てみよう.  

![GuidedFilter_01](Image/GuidedFilter_01.webp)  

うん、いい感じ.ボケてることからも分かる.  
今回は$`I`$と$`p`$のデータは同じものと仮定した.  
この場合はエッジ保持フィルタリングとして作用するようになる.  
実際論文中では$`I`$に画像、$`p`$にマスクとして境界線調整に使ってるとかしてるっぽい.  
まあこれはデータ用意が面倒なのでやってないが、そういう使い方もできるということで.  

今回はこの辺で終わるが、次はこれの高速化Ver.があるっぽいので、それをやろうかなと思う.  

[^1]: [BilateralFilter](./BilateralFilter.md)  
[^2]: [Guided Image Filtering](https://people.csail.mit.edu/kaiming/publications/eccv10guidedfilter.pdf)  
[^3]: [Guided filter](https://w.wiki/e$AA)    