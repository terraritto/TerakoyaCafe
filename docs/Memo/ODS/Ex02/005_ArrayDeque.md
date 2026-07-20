# ArrayDeque
こちらは双方向に実装していくタイプ.  
Queueに似ている部分もあるので見ながら理解していこう.  
まずはデータ.ここはQueueと変わらない.  
```c++
TArray<T> m_data;
int m_start;
int m_size;
```
データの取得.  
今回も管理はリングバッファなので、start位置からoffsetで足して取得するだけ.  
```c++
T Get(int i)
{
    // start位置からのOffsetで算出
    return m_data[(m_start + i) % m_data.m_length];
}
```
SetもGetと同じ方法で位置が分かる.  
なので、この位置にデータを突っ込むだけ.  
返り値は今まで入ってた値を返すだけ.  
```c++
T Set(int i, T x)
{
    // start位置からのOffsetで算出
    T y = m_data[(m_start + i) % m_data.m_length];
    m_data[(m_start + i) % m_data.m_length] = x;
    return y;
}
```
そしたらまずは追加.  
今回は追加として位置を指定しての追加となる.  
Stackの時と同じような感じだね.  
```c++
void Add(int i, T x)
```
最初はいつも通りサイズ確認、足りてないならResize
```c++
    // 足りてないなら確保
    if (m_size + 1 > m_data.m_length)
    {
        Resize();
    }
```
そしてデータを突っ込む位置が前からの方が近いかを調べる.  
近い場合はデータを左にずらして開ける.  
ただし、リングバッファとなっていることには注意.  
リングバッファなので、左にずらすとスタート位置も変わる.  
```c++
    if (i < m_size / 2)
    {
        // 手前からの方が距離的に近い場合
        // スタート位置を左にずらしておく
        m_start = (m_start == 0) ? m_data.m_length - 1 : m_start - 1;

        for (int j = 0; j <= i - 1; j++)
        {
            // データを左にずらしてi番目を空けておく
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j + 1) % m_data.m_length];
        }
    }
```
もし右側の方が近いなら右にデータをずらして開ける.  
後ろに追加する場合はスタート位置の前にデータはできないので、ここではstartをずらす必要はない.  
```c++
    else
    {
        // 後ろ空の方が距離的に近い場合
        for (int j = m_size; i < j; j--)
        {
            // データを右にずらしてi番目を空けておく
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j - 1) % m_data.m_length];
        }
    }
```
ArrayStackの時は何処からずらすという概念がなかったので、一番遠い時は全部のデータをずらすことがあった.  
今回はどれだけ多くても半分の量で済むという点が大事.  
こうして上手く計算量を減らしてるということは頭に入れておきたいね.  
最後にデータを入れてサイズを増やせば終わり.  
```c++
    // 空いた場所に入れる
    m_data[(m_start + i) % m_data.m_length] = x;

    // データ数をCountUp
    m_size++;
```
まとめると以下のような感じ.  
```c++
// 追加
void Add(int i, T x)
{
    // 足りてないなら確保
    if (m_size + 1 > m_data.m_length)
    {
        Resize();
    }

    if (i < m_size / 2)
    {
        // 手前からの方が距離的に近い場合
        // スタート位置を左にずらしておく
        m_start = (m_start == 0) ? m_data.m_length - 1 : m_start - 1;

        for (int j = 0; j <= i - 1; j++)
        {
            // データを左にずらしてi番目を空けておく
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j + 1) % m_data.m_length];
        }
    }
    else
    {
        // 後ろ空の方が距離的に近い場合
        for (int j = m_size; i < j; j--)
        {
            // データを右にずらしてi番目を空けておく
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j - 1) % m_data.m_length];
        }
    }

    // 空いた場所に入れる
    m_data[(m_start + i) % m_data.m_length] = x;

    // データ数をCountUp
    m_size++;
}
```
Resizeに関してはArrayQueueと同じ.  
```c++
// サイズ変更
void Resize()
{
    // 2倍確保して代入
    TArray<T> temp(Max(2 * m_size, 1));
    for (int i = 0; i < m_size; i++)
    {
        // リングバッファを考慮しつつ、先頭になるように入れる
        temp[i] = m_data[(m_start + i) % m_data.m_length];
    }
    m_data = temp;

    // データが先頭になったので0にしておく
    m_start = 0;
}
```
最後にRemove.  
これも手前からずらした方がいいか奥からずらした方がいいかで分ける.  
まず最初は手前から,手前はAddと逆で右にずらす.  
そして、startの位置が右にずらした分+1の位置になるので、その分もずらしておく.  
```c++
if (i < m_size / 2)
{
    // 手前からの方が距離的に近い場合
    for (int j = i; 0 < j; j--)
    {
        // データを右にずらしてi番目を潰す
        m_data[(m_start + j) % m_data.m_length] =
            m_data[(m_start + j - 1) % m_data.m_length];
    }

    // 全て右にずれたので、スタート位置もずらす
    m_start = (m_start + 1) % m_data.m_length;
}
```
そして、奥の場合は左にずらしてつぶすだけ.  
Addの場合と同じでstartの位置ずらしはいらない.  
```c++
else
{
    // 後ろ空の方が距離的に近い場合
    for (int j = i; j < m_size - 1; j++)
    {
        // データを左にずらしてi番目を潰す
        m_data[(m_start + j) % m_data.m_length] =
            m_data[(m_start + j + 1) % m_data.m_length];
    }
}
```
後はデータ数を減らして、リサイズ判定をすればOK.  
```c++
    // データ数をCountDown
    m_size--;

    // 余りにも小さい場合は無駄を減らすためshrink
    if (m_data.m_length >= 3 * m_size)
    {
        Resize();
    }
```
これをまとめたのが以下の形.  
```c++
T Remove(int i)
{
    T current = m_data[m_start];

    if (i < m_size / 2)
    {
        // 手前からの方が距離的に近い場合
        for (int j = i; 0 < j; j--)
        {
            // データを右にずらしてi番目を潰す
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j - 1) % m_data.m_length];
        }

        // 全て右にずれたので、スタート位置もずらす
        m_start = (m_start + 1) % m_data.m_length;
    }
    else
    {
        // 後ろ空の方が距離的に近い場合
        for (int j = i; j < m_size - 1; j++)
        {
            // データを左にずらしてi番目を潰す
            m_data[(m_start + j) % m_data.m_length] =
                m_data[(m_start + j + 1) % m_data.m_length];
        }
    }

    // データ数をCountDown
    m_size--;

    // 余りにも小さい場合は無駄を減らすためshrink
    if (m_data.m_length >= 3 * m_size)
    {
        Resize();
    }

    // 消したものを返す
    return current;
}
```
これでDequeも終わり！  