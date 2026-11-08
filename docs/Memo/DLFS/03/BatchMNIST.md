# BatchでまとめてMNISTを推論する  
前回で推論はできたけど、1枚ずつ処理をしてたのでちょっと面倒が多い.  
配列のキャッシュやらなんやらを考えると、まとめて数枚を処理できた方がよいと思えるかと思う.  
このまとめた入力を「バッチ」というので、バッチ処理をするとか言ったりするらしい.  

ではまとめたまま計算はどうすれば？と思うけど、結局そこまで難しくはない.  
現状の計算を見てみよう.  

```math
\begin{split}
& X = 784 \\
& Y_{1} = XW_{1} = 784 \times (784 \times 50) = 50 \\
& Y_{2} = Y_{1}W_{2} = 50 \times (50 \times 100) = 100 \\
& Y = Y_{2}W_{3} = 100 \times (100 \times 10) = 10
\end{split}
```

入力が$`X=784`$で,MNISTは$`28 \times 28`$のサイズの画像なので、これを1列にしたら$`784`$という形状になるわけだ.  
これに重みをかけていくと、結果として$`Y=10`$の$`[0,9]`$の数字に対応した数値がsoftmaxで手に入ることになる.    

では、例えば100枚の入力画像を用意したらどうか？  
この時は$`X=100 \times 784`$となるわけだが、実際に形状がどうなるかを見てみよう.  

```math
\begin{split}
& X = 100 \times 784 \\
& Y_{1} = XW_{1} = (100 \times 784) \times (784 \times 50) = 100 \times 50 \\
& Y_{2} = Y_{1}W_{2} = (100 \times 50) \times (50 \times 100) = 100 \times 100 \\
& Y = Y_{2}W_{3} = (100 \times 100) \times (100 \times 10) = 100 \times 10
\end{split}
```

こうして計算すると、$`Y=100 \times 10`$という形状が手に入ることになる.  
100枚の画像を入力したら100個分のラベルが手に入ることになるため、ちゃんと計算できることになる.  

これを実際に計算してみよう.  
変えるのは処理を行うところだけ！  
まず入力としてバッチサイズを渡せるようにする.  
```c++
void Process(int batchSize);
```
実際の処理で変えるのはまず最初にBatchSize分で分けるだけ.  
NumCppは`nc::Slice`でBatch分で区切ることが可能!!  
```c++
for (int i = 0; i < testImages.numRows(); i+= BatchSize)
{
    auto data = testImages(nc::Slice(i, i+BatchSize), testImages.cSlice());
    data /= 255.0f;
```
形状が変わっただけで計算に関してはそのままでOK!!  
```c++
    data = data.dot(m_weightMap[U"W1"]) + m_weightMap[U"b1"];
    sigmoid.Process(data);
    data = data.dot(m_weightMap[U"W2"]) + m_weightMap[U"b2"];
    sigmoid.Process(data);
    data = data.dot(m_weightMap[U"W3"]) + m_weightMap[U"b3"];
    softmax.Process(data);
```
最後にBatchSize分の最大値を取り出し、BatchSize分のLabelも取ってくる.  
そして、値が合ってるときのみカウントアップすればよい.  
```c++
    auto maxIndex = data.argmax(nc::Axis::COL);
    auto label = testLabels(0, nc::Slice(i, i + BatchSize));
    for (int i = 0; i < maxIndex.size(); i++)
    {
        count += maxIndex[i] == label[i];
    }
}
```
これで完成！  
今回は100枚で処理をしてみよう.  
```c++
NeuralMNISTBatch mnist;
mnist.Initialize();
mnist.Process(100);
```

これで実行した結果は以下のような感じ.  

![BatchMNIST_01](Image/BatchMNIST_01.webp)  

うん、特に変わらず.  
単純に入力が複数になっただけなので、まあそりゃねという感じ.  
今回の内容はこれで一通り終了！  