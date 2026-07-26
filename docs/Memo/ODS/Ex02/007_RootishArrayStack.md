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
これはi番目というのは0~i番目までなので、i+1個のデータがブロック0~bのb+1個のブロックの合計$`\frac{(b+1)(b+2)}{2}`$を超えないようにしてあげればよい.  
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
