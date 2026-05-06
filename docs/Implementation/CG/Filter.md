# フィルタ周りの実装
* Mean Filter  
    `case 0`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)
    memo: [link](../../Memo/CG/Filter/MeanFilter.md)

* Median Filter  
    `case 1`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)
    memo: todo

* 1-Pass Gaussian  
    `case 2`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)
    memo: [link](../../Memo/CG/Filter/OnePassGaussianFilter.md)

* Bilateral Filter  
    Sigma2つ/iter/FilterSizeを設定して適用  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BilateralFilter.h)
    movie: [link](https://youtu.be/WwVQfapRkXw)
    memo: [link](../../Memo/CG/Filter/BilateralFilter.md)

* Sobel Filter  
    エッジ検出フィルタ、中心に近いほど重みが大きい  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SobelFilter.h)
    movie: [link](https://youtu.be/rXzaPR2FDGY)
    memo: [link](../../Memo/CG/Filter/SobelFilter.md)

* Kuwahara Filter  
    油絵っぽいものを作るフィルタ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/KuwaharaFilter.h)
    movie:[link](https://youtu.be/po2Lr4QWV3E)
    memo: [link](../../Memo/CG/Filter/KuwaharaFilter.md)

* SNN Filter  
    絵画調フィルタ、対称にピクセル比較を行っていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SNNFilter.h)
    movie:[link](https://youtu.be/39c-2kxq8YE)
    memo: [link](../../Memo/CG/Filter/SNNFilter.md)

* Bayar Filter  
    カメラで使われているベイヤーフィルター  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BayarFilter.h)
    movie: [link](https://youtu.be/JyxDIZewrMI)
    memo: [link](../../Memo/CG/Filter/BayarFilter.md)