# FXDoG  
さて、フィナーレ.  
最後なので今までの流れをおさらいしよう.  

まずはLoGを実装[^1],その後これを近似した形のDoGを実装した[^2].  
更にこのDoGを再考したということで、XDoGを実装[^3].  
がこの論文ではFXDoGというものが触れられており、その元となるのはFDoG.  
これを理解する一歩目としてETFを実装[^4],これで場の流れを実装できた.  
ETFの場の流れを使うことで、anisotropicにフィルタを作ってフィルタを掛けたのがFDoGの実装[^5].  
そして、このFDoGに対してXDoGを適用したものがFXDoGとなる.  

やることは簡単でFDoGにおいてDoGを使ってた部分をXDoGに変えるだけ.本当にあっけない.  

```c++
double convResult = conv - convK;
convResult = (1.0 - XDoGTau) * conv + XDoGTau * convResult;
```
比較に関してはXDoGのものを使用するようにしてみた.  

```c++
result = result > epsilon ? 1.0 : 1.0 + Tanh(phi * (result-epsilon));
```

これで準備は整った.  
結果は以下である.  

![FXDoG](Image/FXDoG_01.webp)

うん、いい感じ.短いですが、これにて終了！  

[^1]: [LoG(Laplacian of Gaussian) Filter](./LoGFilter.md)  
[^2]: [DoG(Difference of Gaussian) Filter](./DoGFilter.md)  
[^3]: [XDoG(eXtended Difference of Gaussian) Filter](./XDoGFilter.md)  
[^4]: [ETF(Edge Tangent Flow)を実装してみる](./EdgeTangentFlow.md)  
[^5]: [FDoG Filter](./FDoG.md)  