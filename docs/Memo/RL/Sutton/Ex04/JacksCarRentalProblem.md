# Jacks Car Rental Problem
次にやるのは方策反復.  
方策反復は方策の評価を行い、それを改善して次につなげる.  
この繰り返しを最適になるまで繰り返すものである.  
式で表すと次のようになる.  
```math
\begin{equation}
    \begin{split}
    \pi_{0} \stackrel{E}{\to} v_{\pi_{0}} \stackrel{I}{\to} \pi_{1} \stackrel{E}{\to} v_{\pi_{1}} \stackrel{I}{\to} \pi_{2} \stackrel{E}{\to} ... \stackrel{I}{\to}
        \pi_{*} \stackrel{E}{\to} v_{\pi_{*}}
    \end{split}
\end{equation}
```
この式は非常にシンプル、$`\pi`$という方策から価値$`v`$が得られる.  
これを使って改善を行い、また新しい方策$`\pi`$を得るといった感じで精度を上げていく.  
これを実際にジャックのカーレンタル問題という問題で解いていくことで理解していく.  

まずは問題設定から考えていく.  
ジャックは2つの支社のマネージャーで、2つの支社で車を貸している.  
まあ要は2店舗のレンタカー屋を経営してるわけだ.  
レンタカー屋としては仕事としては車を貸すことなので、貸すことになるわけだが、 

* 車を1台貸したら、10ドル貰える
* レンタカー屋に車がなければ貸せない

という条件で報酬がもらえることになる.  
そして、一つのレンタカー屋では20台の車を置ける.  
まずはここまでの定数を用意してみよう.  
```c++
constexpr int MAX_CARS = 20; // 車の最大数
constexpr double RENTAL_CREDIT = 10.0; // レンタルした際の報酬
```
さて、ではどれくらいの頻度で車を借りに来るかだけど、これはポアソン分布に沿うとする.  
```math
\begin{equation}
    \begin{split}
    \frac{\lambda^{n}}{n!}e^{-\lambda}
    \end{split}
\end{equation}
```
これがポアソン分布.  
今回はポアソン分布がどんなものかは置いといて、この確率分布に沿って人が来るんだなくらいで考えとく.  
このポアソン分布は$`\lambda`$で決まるが、今回はそれぞれ$\lambda_{1}=3, \lambda_{2}=4$とする.  
```c++
constexpr int RENTAL_REQUEST_FIRST_LOCATION = 3; // 最初の位置でのレンタル要望の期待値
constexpr int RENTAL_REQUEST_SECOND_LOCATION = 4; // 次の位置でのレンタル要望の期待値
```
借りに来る人数はポアソン分布だが、その日に返しにくる人数もポアソン分布に従うとする.  
こちらはそれぞれ$\lambda_{1}=3, \lambda_{2}=2$とする.  
```c++
constexpr int RETURN_FIRST_LOCATION = 3; // 最初の位置での車返却の期待値
constexpr int RETURN_SECOND_LOCATION = 2; // 次の位置での車返却の期待値
```
また、車は夜に動かすことができる.  
ジャックは2ドルかかるが、片方のレンタカー屋からもう片方のレンタカー屋に移せる.  
2ドルかかっても、次の日に車を貸し出せれば0ドルのところが10ドル入るので、結果8ドル得することになる.  
なので、車がない場合は移動するのも重要な戦略となる.  
ただし、夜の間に動かせる車の数は最大5台である点には注意が必要である.  
```c++
constexpr int MAX_MOVE_CARS = 5; // 夜の間に動かす車の数
constexpr double MOVE_CAR_COST = 2.0; // 1台の車を動かす際のコスト
```
これでジャックのカーレンタル問題の問題設定は終了だけど、一応制約はかけておく.  
ベルマン方程式の割引率は0.9としておく.  
また、ポアソン分布のnに関しては11が最大としておく.  
```c++
constexpr double DISCOUNT = 0.9; // 割引率
constexpr double POISSON_UPPER_BOUND = 11; // ポアソン分布のnの上界
```
これで定数の準備が終わった、実際にこの条件を元に方策反復をしていこう.  
まずは夜に動かせる全行動を定義しておく.  
正の値は1つ目のレンタカー屋からの車の移動.  
逆に負の値は2つ目のレンタカー屋からの車の移動を表す点は意識しておくとよいかも.  
```c++
// 全てのあり得る行動
std::vector<int> actions;
for (int value = -MAX_MOVE_CARS; value <= MAX_MOVE_CARS; value++)
{
    actions.push_back(value);
}
```
そしてポアソン分布の確率の計算、式通りにするだけ.  
```c++
// ポアソン分布の確率
auto CalculatePoissonProbability = [&](int n, double lambda)
    {
        double factorial = 1.0;
        for (int i = n; i > 0; i--)
        {
            factorial *= i;
        }
        return std::pow(lambda, n) * std::exp(-lambda) / factorial;
    };
```
今回は最適な夜の車の移動はどうかというのを可視化することを目標とする.  
そのため、まずは現状の各支所の車の台数とそこの評価値を定義しておく.  
```c++
Array<double> values; values.resize((MAX_CARS + 1) * (MAX_CARS + 1), 0.0);
Array<double> policies; policies = values;
```
そしたらまずは方策評価から、これは単純で今までやってたことと何も変わらない.  
```c++
// policy evaluation
while (true)
{
    Array<double> oldValues = values;
    for (int i = 0; i < MAX_CARS + 1; i++)
    {
        for (int j = 0; j < MAX_CARS + 1; j++)
        {
            int index = i * (MAX_CARS + 1) + j;
            values[index] = ReturnExpected({i,j}, policies[index], values);
        }
    }

    double maxValue = std::numeric_limits<double>::lowest();
    for (int i = 0; i < values.size(); i++)
    {
        maxValue = Max(maxValue, Abs(oldValues[i] - values[i]));
    }

    // 収束と判断したら終了
    if (maxValue < 1e-4)
    {
        break;
    }
}
```
今までと同様でDP方式で`ReturnExpected`で更新を行う、そして収束してたら終了するだけ.  
つまり違う点は`ReturnExpected`の更新が書ければ前と同じとなる.  
次はこれを見ていこう.  

まず最初は夜に車を動かす際のコストを計算する.   
```c++
auto ReturnExpected = [&](const Vec2& state, double action, const Array<double>& currentValue)
    {
        double returnValue = 0.0;

        // 車を動かすためのコスト
        returnValue -= MOVE_CAR_COST * Abs(action);
```
`action`が動かす台数で、`MOVE_CAR_COST=2`は動かす際のコスト
5台動かすなら2x5で10コスト、3台なら2x3で6コストみたいな感じ.  
次に車の台数を動かすので、その分を考慮していく.  
```c++
// 車を動かす
double firstLocationCars = Min(state.x - action, static_cast<double>(MAX_CARS));
double secondLocationCars = Min(state.y + action, static_cast<double>(MAX_CARS));
```
先ほど言った通り、正の場合は1つ目のレンタカー屋から動かす、負の場合は2つ目のレンタカー屋から動かすということだった.  
一つ目のレンタカー屋から動かす場合を試しに考えてみよう.  
`state.x - action`なので、actionが正の場合は車を確かに動かしてるのが分かる.  
そして、もしactionが負の場合は車の数が増えることになる.2つ目のレンタカー屋から持ってくる感じだね.  

さて、次に車を貸し出す方を考えてみよう.  
まずはポアソン分布で使う`n`を全部列挙する.  
ベルマン方程式の計算をするので、全パターンを列挙する必要があるからだね.  
```c++
for (int first = 0; first < POISSON_UPPER_BOUND; first++)
{
    for (int second = 0; second < POISSON_UPPER_BOUND; second++)
    {
        //...
    }
}
```
そしたら次に貸し出す台数`n`と変数$`\lambda`$から確率を計算する.  
```c++
// 確率
double firstProb = CalculatePoissonProbability(first, RENTAL_REQUEST_FIRST_LOCATION);
double secondProb = CalculatePoissonProbability(second, RENTAL_REQUEST_SECOND_LOCATION);
double probability = firstProb * secondProb;
```
そして、現在ある車の台数から変数を用意しておく.  
```c++
double firstLocation = firstLocationCars;
double secondLocation = secondLocationCars;
```
次に貸し出す台数を決定する.  
台数はfirstとsecondじゃない？となるかもだけど、レンタカー屋にあるだけの台数しか貸し出しはできない.  
3台の貸し出しをしたいという客がいても、2台しかレンタカー屋にしかないのと同じである.  
なので、`min`関数を駆使して貸し出せる台数よりは超えないようにしておく.  
そして貸し出した総計に関しても用意しておく.  
```c++
double validRentalFirstLocation = Min(firstLocation, static_cast<double>(first));
double validRentalSecondLocation = Min(secondLocation, static_cast<double>(second));
double totalRentalValidLocation = validRentalFirstLocation + validRentalSecondLocation;
```
最後に報酬の計算と車の台数を反映させておく.  
トータル貸し出し台数は計算してあるので、これに報酬分を掛けるだけである.  
そして、台数はそのまま貸し出し分を引くだけ、何も難しいことはない.  
```c++
double reward = totalRentalValidLocation * RENTAL_CREDIT;
firstLocation -= validRentalFirstLocation;
secondLocation -= validRentalSecondLocation;
```
これで貸し出しの方は終わった.  
次は客からの返却の方を考える.  

まず最初の確率の計算までは貸し出しの時と同じ.  
ただし最終結果は貸し出し時のモノも合わせたものとなる.  
```c++
for (int returnedFirst = 0; returnedFirst < POISSON_UPPER_BOUND; returnedFirst++)
{
    for (int returnedSecond = 0; returnedSecond < POISSON_UPPER_BOUND; returnedSecond++)
    {
        // return時の確率も考慮
        firstProb = CalculatePoissonProbability(returnedFirst, RETURN_FIRST_LOCATION);
        secondProb = CalculatePoissonProbability(returnedSecond, RETURN_SECOND_LOCATION);
        double returnProbability = probability * firstProb * secondProb;

        // ...
    }
}
```
そして返却時の台数計算.  
返却されてもおける台数は決まっている.  
そのため、置ける最大台数を超えないようにするのを忘れずにする.  
```c++
int returnedFirstLocation = firstLocation + returnedFirst;
returnedFirstLocation = Min(returnedFirstLocation, static_cast<int>(MAX_CARS));
int returnedSecondLocation = secondLocation + returnedSecond;
returnedSecondLocation = Min(returnedSecondLocation, static_cast<int>(MAX_CARS));
```
最後にいつも通りベルマン方程式を計算すれば終わりとなる.  
```c++
returnValue += returnProbability * (reward + DISCOUNT * currentValue[returnedFirstLocation * (MAX_CARS + 1) + returnedSecondLocation]);
```
これで実際のベルマン方程式で計算もできたので、評価もできた.  
次は方策反復における大事な処理、方策改善に入る.  

さて方策の更新を行っていく.  
方策が安定していれば終わりとなるので、方策が安定しているかを判定するbool値を用意しておく.  
```c++
// policyが安定しているかの判定
bool isPolicyStable = true;
for (int i = 0; i < MAX_CARS + 1; i++)
{
    for (int j = 0; j < MAX_CARS + 1; j++)
    {
        // ...
    }
}
```
まずは変数の用意.  
一つ目はポリシーのindex.  
1つ目のレンタカー屋に存在できる車の台数の0から20.  
2つ目のレンタカー屋に存在できる車の台数の0から20.  
この20x20の400パターンの中のindexの位置を決定する.  
各パターンにおける台数を動かす最適解を今回求めようとしてるので、まあそうなるよねという感じ.  
```c++
int index = i * (MAX_CARS + 1) + j;
```
次にポリシーが安定してるかを決定するために、前回に何台を動かしたかを保持しておく.  
```c++
double oldAction = policies[index];
```
最後に現在のベルマン方程式で計算した期待値を算出したものを保持するために、保持できるリストも用意しておく.  
```c++
Array<double> actionReturns;
```
そしたら次に評価は安定しているので、安定した期待値を計算して値を保持する.  
ただし、存在する台数よりも多くは動かせないため、その場合は最小の期待値を保持するようにしておく.  
最小の期待値は`std::numeric_limit<T>().lowest()`を使って計算を行う.  
```c++
for (const double& action : actions)
{
    bool isRangeX = ((0 <= action) && (action <= i));
    bool isRangeY = ((-j <= action) && (action <= 0));
    if (isRangeX || isRangeY)
    {
        actionReturns.push_back(ReturnExpected({ i,j }, action, values));
    }
    else
    {
        actionReturns.push_back(std::numeric_limits<double>().lowest());
    }
}
```
これで各アクションの期待値の計算が終わった.  
なので、期待値の中で最大のもの、つまり一番夜に台数を動かした際に収益の高いものを選択する.  
そして、一番収益の高い台数を保持しておく.  
```c++
int newAction = actions[std::distance(actionReturns.begin(), std::max_element(actionReturns.begin(), actionReturns.end()))];
policies[index] = newAction;
```
最後にポリシーが安定しているかを前回の値と比較して確認する.  
安定していなければ、フラグをおろしておく.  
```c++
if (isPolicyStable && oldAction != newAction)
{
    // まだ安定してない
    isPolicyStable = false;
}
```
最後に安定していれば終わり、もし安定してなければまた評価と更新をすることになる.  
最後に結果を見てみる.  

ここで画像用意.  
結果を見ていこう.  
横軸は2つ目のレンタカー屋の台数.  
縦軸は1つ目のレンタカー屋の台数.  
下に行くと20台上が0台,右が20台で左が0台となる.  
この時の中の値は動かす台数を表している.  
青が小さい値で、赤が大きい値となるため、青は負の台数で赤は正の台数となる.  
こうしてみると台数が極端に偏ってる場合に台数の移動が起こっており、中央のどちらもある程度揃ってる場合は車の移動は起こらないようになっている.  
そして縦軸の方は正の値となっており、横軸は負の値となる.  
これは移動としても正しい.  
1つ目のレンタカー屋に台数が集中してるなら移動させるべきだし、2つ目に固まってても同じ.  
ちゃんと台数を動かす際の最適解が計算できているのが分かった.  