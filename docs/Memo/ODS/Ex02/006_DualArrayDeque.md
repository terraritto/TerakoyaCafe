# DualArrayDeque  
次のArrayは2つの配列を使って構成する配列である.  
つまり、まずは変数としてArrayを2つ用意する.  
```c++
ArrayStack<T> m_front;
ArrayStack<T> m_back;
```
配列の大きさは2つのArrayのサイズを合計するだけでOK.  
```c++
int Size() const
{
    return m_front.Size() + m_back.Size();
}
```
まずはGetから見ていこう.  
今回の配列はfrontとbackという二つの配列がくっついてるような形.  
なので、indexで参照する際はfrontのsize内ならfrontから取得.  
それよりも後ろならbackから取得するだけでOK.  
ただし、backから取得する際はindexはfrontから引いた形になる.  
ここは間違えると配列範囲外となっちゃうので注意.  
```c++
// 取得
T Get(int i)
{
    if (i < m_front.Size())
    {
        // front内に収まってる場合
        // 後ろから先頭に埋めるので、配列の後ろを先頭にするように取得
        return m_front.Get(m_front.Size() - i - 1);
    }
    else
    {
        // frontをあふれる場合、backにデータが入ってる
        // こちらは先頭から入ってるので、先頭のindexが0からなるように調整して取得
        return m_back.Get(i - m_front.Size());
    }
}
```
これが分かればsetは簡単、値を入れるようにするだけでよい.  
```c++
T Set(int i, T x)
{
    if (i < m_front.Size())
    {
        // front内に収まってる場合
        // 後ろから先頭に埋めるので、配列の後ろを先頭にしてセット
        return m_front.Set(m_front.Size() - i - 1, x);
    }
    else
    {
        // frontをあふれる場合、backにデータが入ってる
        // こちらは先頭から入ってるので、先頭のindexが0からなるように調整してセット
        return m_back.Set(i - m_front.Size(), x);
    }
}
```
データの追加に関してもGetとSetと同じような感じでOK.  
ただし、配列自体のサイズに関しては今まではResizeでやっていたけど、今回はBalanceという関数で行う.  
やりたいこととしてはResizeと変わらないけど、配列が2つなのでちょっと面倒.  
Balanceは先にやるとして、とりあえずコードを載せておく.  
```c++
void Add(int i, T x)
{
    // 位置に応じて前か後ろかを決定後に追加
    if (i < m_front.Size())
    {
        m_front.Add(m_front.Size() - i, x);
    }
    else
    {
        m_back.Add(i - m_front.Size(), x);
    }

    // 調整
    Balance();
}
```
そしてRemoveの場合、これもAddとほぼ同じ.  
削除してBalanceするだけ.  
```c++
T Remove(int i)
{
    T current;

    // 前後どちらなのかを決定後削除
    if (i < m_front.Size())
    {
        current = m_front.Remove(m_front.Size() - i - 1);
    }
    else
    {
        current = m_back.Remove(i - m_front.Size());
    }

    // 調整
    Balance();

    // 消したものを返す
    return current;
}
```
そしたら最後にBalanceの処理を見ていく.  
まずは前と後ろの配列に均等に分配されてるかを判定する.  
分配されてない場合はデータの確保を行う.  
```c++
int frontSize = m_front.Size();
int backSize = m_back.Size();

// 前と後ろで三倍の差がついていない場合は処理しない
// そこまでの差がなければデータが均等に分けられているという判断
if (!(3 * frontSize < backSize || 3 * backSize < frontSize))
{
    return;
}
```
もし確保が必要な場合は半分のサイズを計算しておく.  
```c++
// 半分を算出
int fullSize = frontSize + backSize;
int halfSize = fullSize / 2;
```
まずは新しく設定するfront側に前方からデータを詰める.  
```c++
// 前に半分詰めたデータを用意
TArray<T> tempFront{ Max(2 * halfSize, 1) };
const int nextFrontSize = halfSize;
for (int i = 0; i < nextFrontSize; i++)
{
    // Getで取得して後方から詰めていく
    // ただあくまで先頭はindex=0になるようにする
    // (でないとTArrayのアクセス的に範囲外になる)
    tempFront[halfSize - i - 1] = Get(i);
}
```
そして、back側に後方からデータを詰める.  
```c++
// 後に半分詰めたデータを用意
TArray<T> tempBack{ Max(2 * halfSize, 1) };
const int nextBackSize = fullSize - halfSize;
for (int i = 0; i < nextBackSize; i++)
{
    // Getで取得して前方から詰めていく
    tempBack[i] = Get(halfSize + i);
}
```
こうしてできたデータを新しく突っ込めばOK.  
```c++
//最後にデータを設定
m_front.m_data = tempFront;
m_front.m_size = nextFrontSize;
m_back.m_data = tempBack;
m_back.m_size = nextBackSize;
```
これでDualArrayDequeも終わり!!  