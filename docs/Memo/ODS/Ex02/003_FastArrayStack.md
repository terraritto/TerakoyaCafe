# FastArrayStack  
前回の`ArrayStack`ではコピーをこうしてた.  
```c++
for (int i = 0; i < m_size; i++)
{
    temp[i] = m_data[i];
}
```
悪いわけではないけど、速度的には良くはない.  
Standard Libraryを使えば以下のように書ける.  
```c++
std::copy((&m_data[0]), (&m_data[0]) + m_size, &temp[0]);
```
なんか`memcpy`してる気分になるね.  
また、追加の時は以下のようなコードを書いてた.  
```c++
for (int j = m_size; i < j; j--)
{
    m_data[j] = m_data[j - 1];
}
```  
この逆側の操作に関してもStandard Libraryにある.  
```c++
std::copy_backward((&m_data[0]) + i, (&m_data[0]) + m_size, (&m_data[0]) + m_size + 1);
```
こうして書き換えるだけで、ちょっと速くなる.  
移動のメモリ操作はライブラリに頼るとよいって感じでしょうか.  
こうして書き換えたら`FastArrayStack`の出来上がり.  