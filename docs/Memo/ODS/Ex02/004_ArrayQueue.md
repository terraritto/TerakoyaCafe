# ArrayQueue
単純なQueueの実装をしていく.  
この実装に関してはRing Bufferのような感じで実装をする.  
以前にOSの動画で入力で使ったあの構造と同じだね.  
まずデータを入れる箱と開始位置,そして現在あるデータのサイズを用意.  
開始位置はリングバッファの開始位置で、ここから後にぐるっとなるようなデータ配置にするためのものである.  
```c++
protected:
    TArray<T> m_data;
    int m_start;
    int m_size;
```
まずは追加から.  
今までと同じ通りにまずデータサイズ的に問題ないかを判定する.  
長さが足りてないなら再確保をする.  
```c++
// 足りてないなら確保
if (m_size + 1 > m_data.m_length)
{
    Resize();
}
```
そして、データの追加.  
剰余を取って、開始位置からサイズをぐるっと回るように配置していく.  
```c++
// スタート位置を考慮しつつ、追加
// リングバッファのようなものなので、ずらしはしなくてよい
m_data[(m_start + m_size) % m_data.m_length] = x;
```
最後にデータをカウントアップすれば終わり.  
```c++
// データ数をCountUp
m_size++;
```
ここまでをまとめたのが以下である.  
```c++
void Add(T x)
{
    // 足りてないなら確保
    if (m_size + 1 > m_data.m_length)
    {
        Resize();
    }

    // スタート位置を考慮しつつ、追加
    // リングバッファのようなものなので、ずらしはしなくてよい
    m_data[(m_start + m_size) % m_data.m_length] = x;

    // データ数をCountUp
    m_size++;
}
```
次は削除の処理を見ていく.  
まずは現状の取り出す値を保持しておく.  
Queueなので先頭を取ればいいね.  
```c++
T current = m_data[m_start];
```
そしたらStartの位置を+1してずらす.  
もし配列の最後尾まで来てる場合は先頭に戻るように剰余で調節する.  
これがリングバッファだね.  
ついでに削除もしたので、データ数も下げておく.  
```c++
m_start = (m_start + 1) % m_data.m_length;
m_size--;
```
配列のサイズに関しても調節をしておく.  
現状の配列のサイズよりも圧倒的にデータ数が少ない場合は縮める.  
今回は3倍のデータ数にならないように適当に調整してる.  
本にはなんで3倍なのかは書いてないけど、まあ`std::vector`とかも現状の2倍とかなので、3倍未満は縮めちゃうのも分かるね.  
```c++
if (m_data.m_length >= 3 * m_size)
{
    Resize();
}
```
最後に現状の消した値を返せばOK.  
```c++
return current;
```
これをまとめたのが以下.  
```c++
// 削除
T Remove()
{
    T current = m_data[m_start];

    // リングバッファであることを考慮しつつ、
    // +1でずらすだけでOK
    m_start = (m_start + 1) % m_data.m_length;

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
最後にサイズ変更.  
これは結構簡単.まずは2倍のデータを確保する.  
サイズに関しては負にならないようにする.  
```c++
TArray<T> temp(Max(2 * m_size, 1));
```
そしたら、リングバッファを考慮しつつデータを移す.  
startの位置からindexを足して、剰余で境界を注意するだけ.  
データを突っ込むのは先頭.  
なので、移した後は`start=0`からまた始まるようになる.  
```c++
for (int i = 0; i < m_size; i++)
{
    // リングバッファを考慮しつつ、先頭になるように入れる
    temp[i] = m_data[(m_start + i) % m_data.m_length];
}
```
あとはデータを移して,startをリセットすれば終了.  
```c++
m_data = temp;
m_start = 0;
```
これをまとめたのが以下.  
```c++
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
これで単純なQueueも完了.  