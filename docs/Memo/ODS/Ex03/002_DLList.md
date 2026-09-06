# DLList: 双方向連結リスト
双方向連結リストの場合は各Nodeが前後のNodeを持つ.  
SLListでは`next`のみであったが、`prev`というNodeも記録しているわけだ.  

```c++
protected:
    struct Node
    {
        T m_value;
        std::shared_ptr<Node> m_prev;
        std::shared_ptr<Node> m_next;
    };
```

今回はListのノード管理は`m_dummy`に任せる.  
sizeに関しては前回と同じため省略.  
```c++
protected:
    std::shared_ptr<Node> m_dummy;
    int m_size;
```

Nodeはindexでdummy内から取得可能.  
ただし、辿り方としては前と後ろの近い方から探索するようにする.  
これは`prev`が追加されたことによりできるようになったことだね.  
`next`のみだと前からしか辿れないので、明らかに双方向連結リストの利点といえそう.  
```c++
// Nodeの取得
std::shared_ptr<Node> GetNode(int i)
{
    std::shared_ptr<Node> p;
    if (i < m_size / 2)
    {
        // 前の方が近いので、前から辿る
        p = m_dummy->m_next;
        for (int j = 0; j < i; j++)
        {
            p = p->m_next;
        }
    }
    else
    {
        // 後ろの方が近いので、後ろから辿る
        p = m_dummy;
        for (int j = m_size; i < j; j--)
        {
            p = p->m_prev;
        }
    }

    return p;
}
```

Nodeが取れさえすれば、GetとSetの実装は簡単.  
`GetNode`でNodeを取得して、そこの値に対してGet/Setするだけである.  
```c++
T Get(int i)
{
    GetNode(i)->m_value;
}

T Set(int i, T x)
{
    auto u = GetNode(i);
    T y = u->m_value;
    u->m_value = x;
    return y;
}
```

次は追加処理.  
追加に関しては`AddBefore`という関数内で行う.  
これはNodeと値を引数に取る.  
```c++
// 追加
void Add(int i, T x)
{
    AddBefore(GetNode(i), x);
}
```

Addの追加は対象のノード`w`の前に追加を行う.  
`w`は`w->prev, w, w->next`のような形式になっている.  
そのため、追加後は`w->prev, u, w, w->next`のような順になればよい.  

そうなると接続関係をちゃんと整理が必要.  
まず`w->prev`,`w->prev->prev`は変わらないが、nextに関しては`u`になるので、`1:w->prev->next=u`と変える.  
uは簡単.`2:u->prev = w->prev`で`3:u->next = w`となる.  
`w`に関しては`next`は特に変わらないけど、`4:w->prev = u`となることも大事.  
最後に`w->next`は何も変わらないので、変更は不要.  
後はこの1\~4の変更を反映すればOK.  
```c++
std::shared_ptr<Node> AddBefore(std::shared_ptr<Node> w, T x)
{
    std::shared_ptr<Node> u = std::make_shared<Node>();
    u->m_value = x;

    // w->prev, u, wのような順で挿入
    u->m_prev = w->m_prev;
    u->m_next = w;

    // wの前なので、prevに登録
    u->m_next->m_prev = u;

    // w->prevの後なので、nextに登録
    u->m_prev->m_next = u;

    // Count Up
    m_size++;

    return u;
}
```

Removeも見ておこう.  
indexを指定して消すが、まずNodeを取得して、ノード自体の削除は別関数に任せる.  
```c++
// 削除
T Remove(int i)
{
    auto w = GetNode(i);
    T x = w->m_value;
    Remove(w);
    return x;
}
```

削除するノードをwとすると、`w->prev,w,w->next`の構造が`w->prev,w->next`の構造になればよい.  
つまり`prev`側は`w->prev->next = w->next`と直接連結.  
`next`側も直接`w->next->prev= w->prev`と直接連結.  
これで依存関係が潰れたので`w`を削除すれば終わり.  
```c++
// 削除処理
void Remove(std::shared_ptr<Node> w)
{
    // prev, w, next　-> prev, next にする
    w->m_prev->m_next = w->m_next;
    w->m_next->m_prev = w->m_prev;
    w.reset();

    // Count Down
    m_size--;
}
```

これで双方向連結リストも終わり！  