# SEList: 空間効率の良い連結リスト  
今度も同じく連結リストを実装するが、やはり前回とはちょっと違いがある.  
1つのNodeが複数のデータを持つという点が違う.  

```c++
struct Node
{
    BDeque<T> m_deque;
    std::shared_ptr<Node> m_prev;
    std::shared_ptr<Node> m_next;

    Node(int block)
        : m_deque(block)
    {}
};
```

BDequeで固定の`block`で確保しただけの個数をNodeは持つことになる.  
DLListは1つのNodeに1つのデータと`prev,next`を持つため、12個のデータがあったら`prev,next`が12個あることになる.  
もし1つのNodeが3つのデータを持てたとしたら、12個のデータに`prev,next`は4個で済む！  
もし`prev,next`合わせて8byteだとすると、SEListはDLListに対して96byteの節約となる.  
意外と計算してみるとこの最適化は侮れないと思う.  

今回のSEListはNodeとサイズ、これはいつも通り.  
追加で`m_block`という1つのNodeが何個のデータを持つかの変数を用意する.  
```c++
protected:
    std::shared_ptr<Node> m_dummy;
    int m_size;
    int m_block;
```

これはコンストラクタで初期化される、つまり最初の段階で1個のBDequeが持つデータ個数が決まる訳である.  

```c++
SEList(int block)
    : m_size(0)
    , m_block(block)
{
    m_dummy = std::make_shared<Node>(block);
    m_dummy->m_next = m_dummy;
    m_dummy->m_prev = m_dummy;
}
```

今回、Nodeの位置を示すクラスとして`Location`というものを用意する.  
現状のNodeとindexが`Location`に記録される.  
これを利用して上手く連結リストの構築を行っていくことになる.  
```c++
class Location
{
public:
    std::shared_ptr<Node> m_node;
    int m_index;

    Location() {}
    Location(std::shared_ptr<Node> node, int index)
        : m_node(node)
        , m_index(index)
    {}
};
```

そしたらまずは`GetLocation`という関数.  
これはどのNodeの何番目のindexにデータがあるかを`location`に記録する.  
やることは簡単でまず先頭から近いかどうかを確認する.  
近い場合は先頭から探索を開始.  
```c++
// NodeデータからLocationデータを構築
void GetLocation(int i, Location& location)
{
    if (i < m_size / 2)
    {
        // 先頭
        auto u = m_dummy->m_next;

        // ...
    }
}
```

先頭の場合、まず現状のDequeのサイズをIndexが超えているかを確認する.  
越えていなければIndexからその分を引き、次のNodeに移る.  
Block単位で飛ばしながら確認していってる感じだね.  
見つかったらNodeとindexを記録すればよい.  
```c++
        // ブロック単位でスキップしていく
        while (u->m_deque.Size() <= i)
        {
            i -= u->m_deque.Size();
            u = u->m_next;
        }

        // 見つかったブロックに対してのindexを入れてやる
        location.m_node = u;
        location.m_index = i;
```

次に後ろから辿る場合.  
後ろからの場合はindexが手前に行くまでNodeを辿っていく.  
もしも見つかったらnodeとindexを記録すればよい.  
Indexは後ろからの場合は調整が必要な点は注意.  
```c++
    else
    {
        auto u = m_dummy;
        int idx = m_size;

        // 後ろから遡っていく
        while (i < idx)
        {
            u = u->m_prev;
            idx -= u->m_deque.Size();
        }

        // idxはiを超えないブロック単位のサイズの合計となってる
        // なので、これをiから引くとindexになる
        location.m_node = u;
        location.m_index = i - idx;
    }
```

ここまでできればGetとSetは簡単だ.  
`GetLocation`でNodeとデータ位置を取得して、そこの値を取得orセットすればよいだけだ.  

```c++
T Get(int i)
{
    Location location;
    GetLocation(i, location);

    // nodeとdeque内のindexが計算されてるので、
    // あとはここを参照すればよい
    return location.m_node->m_deque.Get(location.m_index);
}

T Set(int i, T x)
{
    Location location;
    GetLocation(i, location);

    // nodeとdeque内のindexが計算されてるので、
    // あとはここを参照すればよい
    T y = location.m_node->m_deque.Get(location.m_index);
    location.m_node->m_deque.Set(location.m_index, x);

    return y;
}
```

さて、追加に関してはちょっと面倒.  
まずは一番簡単な末尾への追加.  
これは一番簡単で、埋まってたら新しいノードを生成.  
あとはデータを追加してあげるだけである.  
```c++
// 要素の単純追加
void Add(T x)
{
    auto last = m_dummy->m_prev;

    // もしdequeが埋まってるなら、新しくノードを生成
    if (last == m_dummy || last->m_deque.Size() == m_block + 1)
    {
        last = AddBefore(m_dummy);
    }

    // 追加
    last->m_deque.Add(x);

    // Count Up
    m_size++;
}
```

新しいノードの作成は前回の双方向連結リストと同じ方法.  
ノードを新しく作って、最後尾の手前に接続してあげればよい.  
```c++
std::shared_ptr<Node> AddBefore(std::shared_ptr<Node> w)
{
    std::shared_ptr<Node> u = std::make_shared<Node>(m_block);

    // w->prev, u, wのような順で挿入
    u->m_prev = w->m_prev;
    u->m_next = w;

    // wの前なので、prevに登録
    u->m_next->m_prev = u;

    // w->prevの後なので、nextに登録
    u->m_prev->m_next = u;

    return u;
}
```

さて、ここまでのは最後尾への追加である.  
ここから実装するのはindexを指定して値を追加するものである.   
この処理で一番簡単なのは末尾への追加.  
先ほどの処理を呼び出すだけで終わり.  
```c++
// 追加
void Add(int i, T x)
{
    // 末尾追加
    if (i == m_size)
    {
        Add(x);
        return;
    }

    // ...

}

さて、データをずらすパターンだけど,これには3パターンがある.  

1. ブロック内に空きがある
2. `block`回の間に全ブロックが埋まってることが分かる
3. `block`回の間に空きが見つからない  

この3パターンに分けることで上手く空きを作ることになる.  
ということで、まずはi番目の`location`を取得して、データを追加したい位置を取得する.  
```c++
    // 位置データ構築
    Location location;
    GetLocation(i, location);

    // 現在のブロックノード
    auto u = location.m_node;
    int r = 0;
```

そしたらBlock回以内に空きがあるかを探索.  
ブロック回数を超えるか、末尾にたどり着くかを達成するまで埋まってるブロックをカウントする.  
```c++
    // 空きのあるノードを探索していく
    // 探索回数はblock数という制限
    while (r < m_block && u != m_dummy && u->m_deque.Size() == m_block + 1)
    {
        u = u->m_next;
        r++;
    }
```

ここまでで1\~3の条件が出揃っていることになる.  
そのため、ここからは判定のターン.  

まず3のパターン、`block`回までに空きが見つからない場合.  
この時は後々のことを考えてデータをフラットにする.  
```c++
    // 3のパターン、block回までに到達できないので、
    // 後のことを考えてデータをflatにする
    if (r == m_block)
    {
        // flatに
        Spread(location.m_node);

        // flatにした影響で空いてるので、ずらしをしないようにマーク
        u = location.m_node;
    }
```

先にフラットにする処理を見ておこう.  
やることは簡単で、追加したいノードから`block`分先のノードを見つける.  
見つけた後はノードを追加してデータを均等にする.  
例として、3blockが全部埋まってる場合は4(個)x3(ブロック)=12個が埋まってる状態である.  
このとき4blockに追加をして、3(個)x4(ブロック)=12個にデータを整形する.  
こうすることで各ブロックに1つだけ空きがある状態になるわけだ.  
これが今回する`Spread`の処理である.  
```c++
void Spread(std::shared_ptr<Node> u)
{
    auto w = u;

    // 埋まってるノードの最後尾に行く
    // 埋まってるかの探索はblock分しかしてないので、block分だけ辿ればよい
    for (int j = 0; j < m_block; j++)
    {
        w = w->m_next;
    }

    // 最後尾にノードを足す
    w = AddBefore(w);

    // 各Blockを1つだけ空いた状態にする
    while (w != u)
    {
        // Blockの個数になるようにずらす(つまり、各ブロック1つ空きが出る状態にする)
        while (w->m_deque.Size() < m_block)
        {
            w->m_deque.Add(0, w->m_prev->m_deque.Remove(w->m_prev->m_deque.Size() - 1));
        }

        // 手前にずらす
        w = w->m_prev;
    }
}
```
後々同じ場所にデータを追加したい場合、こうしておけばずらすだけで済むのでありがたいわけだね.  

さて、そして次に2の場合.  
この時はblock回に到達してない場合だが、この際は3のように空きは作らず単純に後ろのブロックにNodeを追加するだけで済ませる.  
多分`block`回まではデータをずらすのを許容してるために,こういう処理になってるのかなと思う.  
```c++
    // 2のパターン,block回の間に末尾にまで到達してるので、特にflatにはしない
    // データを追加できるようにノード確保
    if (u == m_dummy)
    {
        u = AddBefore(u);
    }
```

さて、3に関しては全てのノードに空きがある状態なのでOK.  
1,2の場合は空きがないノードが存在している可能性があるので空きを作る必要がある.  
これは`ArrayStack`の時みたいに、追加したいところにデータを入れるためにデータをずらすのと全く同じ処理.  
```c++
    // 1,2のパターン,3は同じノードになってるので別段この処理はしない
    // 単純にデータをシフトさせるだけ
    // データシフト後はデータを挿入したいノードが1つ空いてる状態になる
    while (u != location.m_node)
    {
        // 前のブロックから1つ消して、今のブロックに1つ移す
        // つまり、前のブロックが1つ空いた状態
        u->m_deque.Add(0, u->m_prev->m_deque.Remove(u->m_prev->m_deque.Size() - 1));

        // 前のブロックへ
        u = u->m_prev;
    }

```

ここまで来たら1\~3のすべてにおいて空きが1つはある状態になってる.  
なので、単純に追加をしてあげれば終了.  
```c++
    // 空いてるのでデータを追加すればOK
    u->m_deque.Add(location.m_index, x);

    // Count Up
    m_size++;
```

追加だけでも結構大変だど、もう一つ削除がある.  
これもAddと同じような手順でやっていく必要がある.  
ノードの削除処理に関しては簡単で、DLListの場合と同じである.  
```c++
// 削除処理
void Remove(std::shared_ptr<Node> w)
{
    // prev, w, next　-> prev, next にする
    w->m_prev->m_next = w->m_next;
    w->m_next->m_prev = w->m_prev;
    w.reset();
}
```

さて、削除に関してもAddと同じように3パターンに分ける.  

1. ブロック内に`block-1`個より多くのデータがあるノードが見つかる
2. `block`回の間に末尾にたどり着く
3. `block`回の間に末尾にたどり着かず、`block-1`個より多くのデータのあるノードが見つからない  

要は削除時に`block-1`個となるようにデータを調整するわけだ.  
なので`block`個のデータがあれば、`block-1`個に調整するようにする.  

ということでまずはAddと共通の処理、1\~3のための判定を行う.  
```c++
// 削除
T Remove(int i)
{
    // 位置データ構築
    Location location;
    GetLocation(i, location);

    T y = location.m_node->m_deque.Get(location.m_index);

    // 現在のブロックノード
    auto u = location.m_node;
    int r = 0;

    // block個あるノードを探索していく
    // 探索回数はblock数という制限
    while (r < m_block && u != m_dummy && u->m_deque.Size() == m_block - 1)
    {
        u = u->m_next;
        r++;
    }

    // ...
};
```

そしたらまずは3のパターンから.  
このパターンは全部が`block-1`個となってるため、上手くデータをずらして`block-1`個と`block`個の山ができるように調整をする.  
```c++
    // 3のパターン、データの整地を行う
    if (r == m_block)
    {
        // flatに
        Gather(location.m_node);
    }
```

この山を作る処理が`Gather`であるが、これを見ていこう.  
ここでデータが`m_block-1`個となる.  
例えば`m_block-1=2`とすると、`{1,2},{1,2},{1,2}`というデータになる.  
この時データを移していくと  
`{1,2},{1,2},{1,2}`→`{1,2,1},{2},{1,2}`→`{1,2,1},{2,1},{2}`→`{1,2,1},{2,1,2},{}`  
のような感じで、全部が`block`個になるように調整をする.  
こうすると一番後ろが絶対に空になるので、消してしまう.  
これを行うのが`Gather`という訳だ.  
```c++
		void Gather(std::shared_ptr<Node> u)
		{
			auto w = u;

			// b個になるように調整
			// (b-1)個の箱がb個 -> b個の箱が(b-1)個 となるようにしてる
			for (int j = 0; j < m_block - 1; j++)
			{
				// b個になるまで後ろから詰めていく
				while (w->m_deque.Size() < m_block)
				{
					w->m_deque.Add(w->m_next->m_deque.Remove(0));
				}

				w = w->m_next;
			}

			// b-1個に圧縮したので、一つ空きが出る
			// なので一つ消す
			Remove(w);
		}
```

さて,ここまでできれば1\~3のどれもデータを消しても問題ない状態となる.  
そのためここでデータを削除する.  
データを削除した後はデータをずらして埋め合わせをしておく.  
例えば、`{1,2},{3,4,5}`というデータがあって、2を消すとする.  
`{1},{3,4,5}`となるわけだが、これをずらすと`{1,3},{4,5}`となることになる.  
先ほど言った`block-1`個になるように調整してるわけだ.  
```c++
    // 削除
    u = location.m_node;
    u->m_deque.Remove(location.m_index);

    // 削除によりblock-1個になってるので、埋め合わせをしていく
    // 単純に配列を左にずらしていくイメージ
    while (u->m_deque.Size() < m_block - 1 && u->m_next != m_dummy)
    {
        // ずらし
        u->m_deque.Add(u->m_next->m_deque.Remove(0));

        // 次のブロックへ
        u = u->m_next;
    }
```

そして、移動した結果末尾が空になってるならもういらないので、削除をしておく.  
これで完成！  
```c++
    // 末尾が空なら必要ないので、ノードを消してしまう
    if (u->m_deque.Size() == 0)
    {
        Remove(u);
    }

    // Count Down
    m_size--;

    return y;
```

こうしてSEListが完成した！  
1つのNodeに複数のデータをまとめたことで、空間的に効率は上がるが、その分データの一貫性を保つのが大変になったなという気分だ.  