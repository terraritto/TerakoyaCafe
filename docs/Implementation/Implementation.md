# 実装まとめ
## Rendering
### Basic
* Bresenham's Algorithm  
    ブレゼンハムのアルゴリズム、直線を引いていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/BresenhamSample.h) 
    movie: まだ

* Edge Function(Triangle)  
    Edge Functionを利用して三角形を塗りつぶす.パターン1の部分を参照  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/TriangleEdgeFunctionSample.h) 
    movie: まだ

* Edge Function(多角形)  
    Edge Functionを利用して多角形を塗りつぶす  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/PentagonEdgeFunctionSample.h) 
    movie: まだ

* Edge Function(Triangle)  
    塗りつぶしの計算量を減らすために賢く探索.パターン3の部分を参照  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/TriangleEdgeFunctionSample.h) 
    movie: まだ

### Light
* Directional Light  
    EvaluateDirectional部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: [link](https://youtu.be/yaIO93brcOs)

* Point Light  
    EvaluatePointLight部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: [link](https://youtu.be/yaIO93brcOs)

* Spot Light  
    EvaluateSpotLight部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: [link](https://youtu.be/yaIO93brcOs)

* Spot Light(Legarde)  
    EvaluateSpotLightLegarde部分、論文読んでおきたいところ.  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: やるかも？

* Rim Light  
    RimThreshold部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: [link](https://youtu.be/tsWAQKoMb1w)

* 半球ライティング  
    CalculateHemisphereLightの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Light.hlsli) 
    movie: [link](https://youtu.be/AOJtVQ2EHlQ)

### BRDF
* Lambert  
    LambertBRDF部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/BRDFDX12.hlsli) 
    movie: [link](https://youtu.be/C3G-wq_Zqog)

* Phong  
    PhongBRDF部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/BRDFDX12.hlsli) 
    movie: [link](https://youtu.be/C3G-wq_Zqog)

* Blinn-Phong  
    BlinnPhongBRDF部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/BRDFDX12.hlsli) 
    movie: [link](https://youtu.be/C3G-wq_Zqog)

* Blinn  
    BlinnSample,EvaluateBlinnの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/BRDF/CalculateBRDF.hlsli)
    movie: [link](https://youtu.be/IbfRRJ68hEA)

* Cook-Torrance   
    CookTorranceSample,EvaluateCookTorranceの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/BRDF/CalculateBRDF.hlsli)
    movie: [link](https://youtu.be/yJQ5WTc43Ik)

### Filter
* Mean Filter  
    `case 0`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)

* Median Filter  
    `case 1`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)

* 1-Pass Gaussian  
    `case 2`の部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl)
    movie: [link](https://youtu.be/zm-NbJyQamg)

* Bilateral Filter  
    Sigma2つ/iter/FilterSizeを設定して適用  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BilateralFilter.h)
    movie: [link](https://youtu.be/WwVQfapRkXw)

* Kuwahara Filter  
    油絵っぽいものを作るフィルタ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/KuwaharaFilter.h)
    movie: まだ


### AntiAliasing
* SSAA  
    `isUseSSAA`の部分を参照  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/MyFrameworkTest/ForRaster/Simple/SimpleModel/SimpleModelApp.cpp)
    movie: [link](https://youtu.be/Ckjz-Tf6ZJo)

* MSAA  
    `isUseMSAA`の部分を参照  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/MyFrameworkTest/ForRaster/Simple/SimpleModel/SimpleModelApp.cpp)
    movie: [link](https://youtu.be/Ckjz-Tf6ZJo)

* FXAA  
    あまり綺麗にDefineはやってない  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/FXAAPS.hlsl)
    movie: [link](https://youtu.be/dHEjkMiliB0)

### Shadow
* Projection Shadow  
    CalculateProjectionShadowの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Shadow/ShadowCommon.hlsli)
    movie: [link](https://youtu.be/RJvdGrEr5jI)

* Depth Shadow  
    CalculateDepthShadowの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Shadow/ShadowCommon.hlsli)
    movie: [link](https://youtu.be/X9DUtOVK-Vg)

* PCF   
    CalculatePCFShadowの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/Shadow/ShadowCommon.hlsli)
    movie: [link](https://www.youtube.com/embed/PGFffnGFsGk)

### Intersection
* Sphere  
    球の交差  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsSphere.hlsl)
    movie: [link](https://youtu.be/5d0wgHcvQfA)

* Plane
    平面の交差  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsPlane.hlsl)
    movie: [link](https://youtu.be/lA3_zsVZJ-M)

* Triangle(クラメル)  
    三角形の交差(クラメルで係数を強引に計算)  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsTriangle.hlsl)
    movie: [link](https://youtu.be/Y6dpiL0erKk)

### Ray Tracing
* PathTracing  
    PTの実装  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/BRDF/CalculatePT.hlsli)
    movie: [link](https://youtu.be/ThH2pvbiamA)

* NEE  
    コメントでNEEの部分のところ  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/BRDF/CalculatePT.hlsli)
    movie: [link](https://youtu.be/kMtRVntRqr0)

### Shader Techniques
* 集中線  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/ConcentrationLinePS.hlsl)
    movie: [link](https://youtu.be/a1_F3COBZfM)

* ラジアルブラー  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/RadialBlurPS.hlsl)
    movie: [link](https://youtu.be/CG108YfOZN8)

### 小技
* CubeMap(TextureSampleのみ)  
    GetSkyBoxColorFromCubeMapの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/CommonDXR.hlsli)
    movie: [link](https://youtu.be/HEciYLNtlgs)

* SkySphere  
    GetSkyBoxColorFromHdrの部分  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/CommonDXR.hlsli)
    movie: [link](https://youtu.be/yh4wj61TDvw)

## Sampling
* MonteCarlo PI  
    球でのモンテカルロ法  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/MonteCarloPI.h)
    movie: [link](https://youtu.be/C5bOtOeS8Fs)  

* 平均値法  
    平均値の定理にモンテカルロ法を適用して、積分を解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/AverageMethod.h)
    movie: [link](https://youtu.be/2OpPPZhzwOs)  

* 逆関数法  
    指数分布に基づいた分布を逆関数法でサンプルする  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/InverseMethod.h)
    movie: [link](https://youtu.be/mylfKB2XLWk)

* ボックス＝ミュラー法  
    正規分布を一様分布から生成する  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/BoxMuller.h)
    movie: [link](https://youtu.be/AAUwBuFAEH4)

* 棄却サンプリング  
    提案分布を用意することで、欲しい分布を得る  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/RejectionSampling.h)
    movie: [link](https://youtu.be/r-bzF5f4Nnk)

## AI
### 基礎的な？
* StateMachine  
    AIStateとAIComponentあたりがStateMachine  
    実装はForStateフォルダ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04) 
    movie: [link](https://youtu.be/jlgzUT4Swjo)

### 探索アルゴリズム
* BFS  
    BFSComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/8rLAYnmfQQ8)

* DFS  
    DFSComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/8rLAYnmfQQ8)

* GBFS  
    GBFSComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/mpksUzLGSUE)

* A\*  
    AStarComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/mpksUzLGSUE)

* Dijkstra  
    DijkstraComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/vEvdewXai-s)  

## ゲーム木
* Min-Max法  
    MinMaxComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/xKIU9grhNlI)  

* Alpha-Beta法  
    AlphaBetaComponentが実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/GameProgrammingC%2B%2B/Example_04/ForGraph)
    movie: [link](https://youtu.be/xKIU9grhNlI)  

## 迷路アルゴリズム
* 穴掘り法  
    Dig関数が実装  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/GameProgrammingC%2B%2B/Example_04/ForGraph/SearchGrid.cpp) 
    movie: 特になし

## お遊び
* SynthWave  
    synthwaveを適当に書くだけ,30分で書いてみた  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/SynthwavePS.hlsl)
    movie: [link](https://youtu.be/Ja8b40hLYQk)

* AtProto  
    AtProtoで遊んだ奴、Pythonを無理やりSiv3Dで呼び寄せた(強引)  
    Impl: [link](https://github.com/terraritto/BSApp)
    movie: [link](https://youtu.be/T6GnyOzj0A8)

* GameProgrammingC++ Series  
    動画シリーズで進めてるやつ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B)
    movie: 多すぎるので略

* OS Series  
    動画シリーズで進めてるやつ  
    Impl: [link](https://github.com/terraritto/OSProject)
    movie: 多すぎるので略

* PRML  
    PRML関係の実装、1章~4章の内容となります。  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/tree/main/PRML)
    movie: まだ
