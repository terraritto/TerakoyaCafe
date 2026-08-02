# RootishArrayStack  
ブロック単位で確保をしていくArray.  
まずは配列は以下のような構造で構築していく.  
```c++
protected:
    ArrayStack<std::shared_ptr<T[]>> m_blocks;
    int m_size;
```
データのサイズと確保しているブロックで計上をしていく.  
ブロックのデータの持ち方はちょっと特殊で、Block0: 1個 Block1: 2個 Block2: 3個といった風になる.  
つまり、ブロックの個数に対するデータの個数は等差数列を使うと,  
```math
\begin{equation}
    \begin{split}
    1+2+3+...+n = \frac{n(n+1)}{2}
    \end{split}
\end{equation}
```
さて、データのi番目がブロック内にあるかどうかを考える.  
これはi番目というのは0 \~ i番目までなので、i+1個のデータがブロック0\~bのb+1個のブロックの合計$`\frac{(b+1)(b+2)}{2}`$を超えないようにしてあげればよい.  
これを実際に式にして解くと,  
```math
\begin{equation}
    \begin{split}
    & \frac{(b+1)(b+2)}{2} \geq i+1 \\
    & b^2 + 3b - 2i \geq 0
    \end{split}
\end{equation}
```
ここで、これが0になる場合を解くと,  
```math
\begin{equation}
    \begin{split}
    & b^2 + 3b - 2i = 0 \\
    & b=\frac{-3+\sqrt{9+8i}}{2},\frac{-3-\sqrt{9+8i}}{2}
    \end{split}
\end{equation}
```
となる.  
欲しいのは正の値で、bがこれよりも大きい場合なので、  
```math
\begin{equation}
    \begin{split}
    b \geq \frac{-3+\sqrt{9+8i}}{2}
    \end{split}
\end{equation}
```
後はこれをコードに落とし込むと、データの位置iからブロックの位置bに変換するコードが求まる.  
```c++
int IndexToBlockIndex(int i)
{
    double db = (-3 + Sqrt(9 + 8 * i)) / 2.0;
    int b = static_cast<int>(Ceil(db));
    return b;
}
```
ただしCeil関数でbの条件を満たす最小のbとなるように調整をしておく.  
さて、ここまでできればGetとSetは簡単なので、最初にこちらから見ていこう.  
まずはGetから.  
まずindexを現状のブロックに変換を行う.  
```c++
    int block = IndexToBlockIndex(i);
```
こうして変換したBlockのIndexが分かれば、Block内のデータの総数が分かる.  
これは(1)の式と同じように和の計算でわかる.  
こうして全体の総数が分かれば、Blockのスタート位置も計算可能.  
よって以下の計算で現在のブロックの位置が分かる.  
```c++
    // blockの個数分引いた値がStart位置なので,blockの個数分を引く
    int j = i - block * (block + 1) / 2;
    // Getだとindex=0スタートなので、blockをそのまま渡せばOK
    return m_blocks.Get(block)[j];
```
これをまとめると以下の形.  
```c++
// 取得
T Get(int i)
{
    int block = IndexToBlockIndex(i);
    // blockの個数分引いた値がStart位置なので,blockの個数分を引く
    int j = i - block * (block + 1) / 2;
    // Getだとindex=0スタートなので、blockをそのまま渡せばOK
    return m_blocks.Get(block)[j];
}
```
Setの場合も同様の計算で位置を特定する.  
その後に値を入れてあげるだけなので簡単.  
```c++
// セット
T Set(int i, T x)
{
    int block = IndexToBlockIndex(i);
    // blockの個数分引いた値がStart位置なので,blockの個数分を引く
    int j = i - block * (block + 1) / 2;
    // Getだとindex=0スタートなので、blockをそのまま渡せばOK
    T y = m_blocks.Get(block)[j];
    m_blocks.Get(block)[j] = x;
    return y;
}
```
さて、次は追加の対応.今回もずらすということでちょっと面倒な作業が必要.  
最初は配列内にデータを詰められるかを確認.  
ブロック内の要素内にまだ空きがあるなら大丈夫.  
もし無理な場合は`grow`という処理を呼ぶことで拡張を行う.`resize`みたいなもんだね.  
```c++
    int r = m_blocks.Size();
    if (r * (r + 1) / 2 < m_size + 1)
    {
        // ブロック内が埋まってるなら確保
        Grow();
    }
```
そして、データを追加する前にデータをずらす必要がある.  
データを突っ込むので、その位置を開ける処理だね.今までもあったね.  
さて、ずらすのは右にずらすような感じで進めていく.  
ArrayStackのResizeとやってることは同じだね.  
```c++
// Count Up
m_size++;

for (int j = m_size - 1; i < j; j--)
{
    // 配列を右にずらして開けるのと同じイメージ
    Set(j, Get(j - 1));
}
```
こうして空いたらデータを突っ込めばOK  
```c++
// 空いた場所に挿入
Set(i, x);
```
これをまとめたのが以下である.  
```c++
// 追加
void Add(int i, T x)
{
    int r = m_blocks.Size();
    if (r * (r + 1) / 2 < m_size + 1)
    {
        // ブロック内が埋まってるなら確保
        Grow();
    }

    // Count Up
    m_size++;

    for (int j = m_size - 1; i < j; j--)
    {
        // 配列を右にずらして開けるのと同じイメージ
        Set(j, Get(j - 1));
    }

    // 空いた場所に挿入
    Set(i, x);
}
```
さて、そしたら次にgrowを見ておこう.  
これは次のブロックを確保するだけである.  
```c++
void Grow()
{
    // 次のブロック分確保するだけ
    m_blocks.Add(m_blocks.Size(), std::move(std::make_shared<T[]>(m_blocks.Size() + 1)));
}
```
各尾するのはブロック数+1でOK.  
これは実際に以下のように考えればよい.  
1つ目のブロックは1個、2つ目のブロックは2個,3つ目のブロックは3個という風にブロックの中のデータ数は増えていく.  
なので、ブロック数+1で次のブロックを確保してあげればよいとなるわけだね.  

次はRemoveを見ていく.  
削除の場合は左にずらして埋めるだけ、これもArrayStackと同じ感じ.  
```c++
for (int j = i; j < m_size - 1; j++)
{
    // 配列を左にずらして埋めてしまうのと同じイメージ
    Set(j, Get(j + 1));
}

// Count Down
m_size--;
```
そして、ブロックサイズが一つ下のサイズの場合.  
つまり、N個のブロック分配列は用意してるけど、N-1個しかデータが埋まってない場合は配列を小さくしてしまう.  
これは`Shrink`処理で行う.  
```c++
int r = m_blocks.Size();
if ((r - 2) * (r - 1) / 2 >= m_size)
{
    // 一つ下のブロックよりも小さいサイズになったら、
    // 一番上のブロックは解放してしまう
    Shrink();
}
```
ここまでまとめたのが以下の形.  
```c++
T Remove(int i)
{
    T x = Get(i);

    for (int j = i; j < m_size - 1; j++)
    {
        // 配列を左にずらして埋めてしまうのと同じイメージ
        Set(j, Get(j + 1));
    }

    // Count Down
    m_size--;

    int r = m_blocks.Size();
    if ((r - 2) * (r - 1) / 2 >= m_size)
    {
        // 一つ下のブロックよりも小さいサイズになったら、
        // 一番上のブロックは解放してしまう
        Shrink();
    }

    return x;
}
```
ということで最後に`Shrink`を見ていこう.  
ここでやるのは簡単で、データ数的にブロックが削れる場合はデータを開放する.  
そしてblock数を下げて更に同じ判定を繰り返して、開けられるBlockまで全部開放していくだけである.  
まあ現状の実装だとRemoveでしかデータが消えず、ShrinkはBlock毎にやるので一回しか起こらないとは思うけども...  
```c++
void Shrink()
{
    int r = m_blocks.Size();

    // blockが最下層ではない+まだ空けられるblockがある
    // この二つを満たす場合は順に空けていく
    while (r > 0 && (r - 2) * (r - 1) / 2 >= m_size)
    {
        m_blocks.Remove(m_blocks.Size() - 1).reset();
        r--;
    }
}
```
さて、これでRootishArrayStackも終わり.  
これで2章も終了!!次からは3章である.  