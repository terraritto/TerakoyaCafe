# ArrayStack  
次はArrayStack,スタックと書いてある通り、スタックである.  
このStackは前回の`Array<T>`を使って定義をする.  
長さのnも変数として持たせる.  
```c++
protected:
    TArray<T> m_data;
    int m_size;
```
基本的にGetter/Setterがある.  
値を取るか、データをセットするかの良くあるやつ.  
ただし、Setの場合は前に入ってた値を返すようにしとく.  
```c++
// Getter
T Get(int i)
{
    return m_data[i];
}

// Setter
T Set(int i, T x)
{
    T y = m_data[i];
    m_data[i] = x;

    // 以前の値を返す
    return y;
}
```
勿論配列のサイズも簡単、`m_size`を返すだけ.  
```c++
int Size() const { return m_size; }
```
データの追加は以下のような感じ.  
まずそもそもデータサイズ的にまだ突っ込めるかを確認.  
もうサイズ的に突っ込めない感じだったら,メモリの確保を行う.  
```c++
if (m_size + 1 > m_data.m_length)
{
    Resize();
}
```
そして、データをずらして、i番目に入れられるようにデータを開ける.  
```c++
for (int j = m_size; i < j; j--)
{
    m_data[j] = m_data[j - 1];
}
```
ずらす方向は右方向で確定.  
なので、100個データがあって,i=1に突っ込みたい場合を考える.  
この時は99個のデータを右にずらす必要があるので、今回の実装は結構非効率ではある.  
逆にスタックみたいに後ろに突っ込むだけであれば、1つずらすだけなので、非常に高速.  
ここまで、できたらデータを入れる場所が確保できてるので突っ込んで、データのサイズを1つ増やしておく.  
```c++
    m_data[i] = x;
    m_size++;
```
最後に全部まとめておく.  
```c++
void Add(int i, T x)
{
    if (m_size + 1 > m_data.m_length)
    {
        Resize();
    }

    for (int j = m_size; i < j; j--)
    {
        m_data[j] = m_data[j - 1];
    }

    m_data[i] = x;
    m_size++;
}
```
次に先ほどの再確保となる`Resize`を見ておこう.  
リサイズ時は2倍のメモリを確保する.`std::vector`とかもこんな感じで2倍の確保をしてたはず.記憶が正しければ.  
```c++
TArray<T> temp(Max(2 * m_size, 1));
```
確保ができたらデータを移す必要がある.  
これは簡単で片っ端から入れるだけ.  
```c++
for (int i = 0; i < m_size; i++)
{
    temp[i] = m_data[i];
}
```
これでデータも移し終えたので、後は反映させればOK.  
```c++
    m_data = temp;
```
これでリサイズでメモリ確保が完了した.  
全体はこんな感じ.  
```c++
void Resize()
{
    TArray<T> temp(Max(2 * m_size, 1));
    for (int i = 0; i < m_size; i++)
    {
        temp[i] = m_data[i];
    }
    m_data = temp;
}
```
最後に削除を見よう.  
消すIndexはわかってるので、そこをなくすように左にずらすだけ.  
```c++
    // 配列内を左シフトして消す
    for (int j = i; j < m_size - 1; j++)
    {
        m_data[j] = m_data[j + 1];
    }
```
消したのでデータサイズを縮めておく.  
```c++
    m_size--;
```
最後に配列がある程度小さくなってたら、配列を縮めておく.  
```c++
    // 余りにも小さい場合は無駄を減らすためshrink
    if (m_data.m_length >= 3 * m_size)
    {
        Resize();
    }
```
すべてまとめると以下の感じ.  
```c++
T Remove(int i)
{
    T current = m_data[i];

    // 配列内を左シフトして消す
    for (int j = i; j < m_size - 1; j++)
    {
        m_data[j] = m_data[j + 1];
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
これでStackも終わり.  