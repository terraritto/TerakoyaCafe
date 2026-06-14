# Array  
固定配列,確保するだけしてそのあとは一切動的な確保をしない感じ.  
内容としては`std::array`とかが近いかな.  
やることは簡単で、長さを受け取ってメモリを確保するだけ.  
```c++
TArray(int length)
    : m_length(length)
{
    m_data = std::make_unique<T[]>(m_length);
}
```
あとは`operator[]`で参照可能.  
```c++
T& operator[](int i)
{
    assert(0 <= i && i < m_length);
    return m_data[i];
}
```
そして、`operator=`で代入もできるけど、これをしたら元の方は機能しなくなる.  
完全にデータを移す感じの実装となる.  
```c++
TArray<T>& operator=(TArray<T>& value)
{
    m_data.reset();

    // 代入後は機能しない想定
    m_data = std::move(value.m_data);
    m_length = value.m_length;
    value.m_length = 0;

    return *this;
}
```
最初はこれくらい,特に難しいことはないね.  