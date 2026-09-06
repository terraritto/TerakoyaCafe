# SLList: 単方向リスト  
3章は連結リスト,Nodeをどう繋ぐかみたいなお話.  

一番最初はまずNodeを定義する.  
Nodeは単純で値と繋がってる次のノードを持つようなものとなる.  
単方向なので、次のノードのみを持つように`m_next`のみを持つわけだ.  
```c++
class Node
{
public:
    Node(T value)
        : m_value(value)
        , m_next()
    { }

public:
    T m_value;
    std::shared_ptr<Node> m_next;
};
```

Listが持つのはHeadとTailのNode.  
後はsizeを保持しておく.  
```c++
protected:
    std::shared_ptr<Node> m_head;
    std::shared_ptr<Node> m_tail;
    int m_size;
```

Sizeに関してはそのまま返せばOK.  

```c++
int Size() const { return m_size; }
```

追加に関しては簡単.  
まずはノードを作って`u`に格納.  
その後、現状のheadを`u`の`m_next`として、headが`u`になるようにする.  
```c++
T Push(T x)
{
    std::shared_ptr<Node> u = std::make_shared<Node>(x);

    // Pushは頭に繋げるため、head側からつける
    u->m_next = m_head;
    m_head = u;

    // ...
}
```

特殊条件として、sizeが0のときはデータがない.  
この時はtailに関しても`u`を設定しておく.  
あとはSizeを+1しておけばOK.  
```c++
    // 初回のみTailにもつけて参照できるように
    if (m_size == 0)
    {
        m_tail = u;
    }

    // Count Up
    m_size++;

    return x;
```

削除は簡単.  
PopはHead側から削除を行うため、headの値を`u`に保持して、headを`u->m_next`にするだけ.  
あとは`u`を解放して、sizeを下げるのみ.  
ただし、sizeが0ならTailにもデータがある状態なので、こっちにも保持しているのを開放する.  
`std::shared_ptr<T>`なので解放しないと残っちゃう.  
```c++
// 削除
T Pop()
{
    if (m_size == 0) { return T{}; }

    T x = m_head->m_value;

    // head側から削除なので、
    // headをnextに付け替えておく
    auto u = m_head;
    m_head = u->m_next;

    // 解放
    u.reset();

    // サイズが0の場合tailにも同じ値が入ってるので、
    // ここも削除しておく
    if (--m_size == 0)
    {
        m_tail.reset();
    }

    return x;
}
```

Removeに関しては全く同じため、Popを呼び出すのみにしてみた.  
```c++
// 削除
T Remove()
{
    // Popと同じ処理
    return Pop();
}
```

最後にAdd,PushはHeadから追加してたのに対して、AddはTailから追加する.  
そこが違うだけなので、Tailの`m_next`に登録して、Tailを更新するだけ.  
```c++
// 追加
bool Add(T x)
{
    std::shared_ptr<Node> u = std::make_shared<Node>(x);

    if (m_size == 0)
    {
        // 初回のみHeadにも付ける
        m_head = u;
    }
    else
    {
        // 今の最後尾の次にマーク
        m_tail->m_next = u;
    }

    // 最後尾に追加
    m_tail = u;

    // Count Up
    m_size++;

    return true;
}
```

これで単純な単方向リストが完成した.  