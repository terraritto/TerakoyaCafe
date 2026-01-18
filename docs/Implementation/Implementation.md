# 実装まとめ
## Rendering
### Basic
* Bresenham's Algorithm  
    ブレゼンハムのアルゴリズム、直線を引いていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/BresenhamSample.h) 
    movie: [link](https://youtu.be/mRRQs_vFNXk)

* Cohen-Sutherland Algorithm  
    クリッピング、矩形限定  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/CohenSutherlandSample.h) 
    movie: [link](https://youtu.be/g4Ix0TcBJ-Q)

* Cyrus-Beck Algorithm  
    クリッピング、多角形もいける！  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/CyrusBeckSample.h) 
    movie: [link](https://youtu.be/DpWlDtt9D2s)

* Surtherland-Hodgman Algorithm  
    三角形を矩形内に収まるようにクリッピング  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/SurtherlandHodgemanApp.cpp) 
    movie: [link](https://youtu.be/ttAN5tCy8_8)

* Edge Function(Triangle)  
    Edge Functionを利用して三角形を塗りつぶす.パターン1の部分を参照  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/TriangleEdgeFunctionSample.h) 
    movie: [link](https://youtu.be/uHUEoTq2w10)

* Edge Function(多角形)  
    Edge Functionを利用して多角形を塗りつぶす  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/PentagonEdgeFunctionSample.h) 
    movie: [link](https://youtu.be/uHUEoTq2w10)

* Edge Function(Triangle)  
    塗りつぶしの計算量を減らすために賢く探索.パターン3の部分を参照  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/TriangleEdgeFunctionSample.h) 
    movie: [link](https://youtu.be/k8z1MI6KRX8)

* CG座標変換  
    座標変換、モデル座標系->ワールド座標系->ビュー座標系->投影座標系->スクリーン座標系の流れ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/VertexProcess.h) 
    movie: [link](https://youtu.be/Ut00Yn21Ohk)

* 法線ベクトル法  
    dot計算で省くだけ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/HiddenSurfaceRemovalNormal.h) 
    movie: [link](https://youtu.be/ubP8RBPaGCg)

* Z-Sort法  
    Z軸の位置をソートして、画家のアルゴリズムをやるだけ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/RemovalFace/RemovalFaceZSort.cpp) 
    movie: [link](https://youtu.be/XhdNgjRiu3k)

* DepthBuffer法  
    メモリは消費するが、上手くデプスを解決  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/RemovalFace/RemovalFaceDepthBuffer.cpp) 
    movie: なし

* グローシェーディング  
    頂点でライティングをしておこう  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/Shading/GourandShadingApp.cpp) 
    movie: なし

* フォンシェーディング  
    ピクセルシェーダの礎  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/Shading/PhongShadingApp.cpp) 
    movie: なし

* ガウシアンスプラッティング可視化  
    ガウシアンスプラッティングを可視化していく、SH以外はできてるはず  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/App/GS/GSVertexApp.cpp) 
    movie: [link](https://youtu.be/GjpH0YqK9Fs)

### Fractal
* コッホ曲線  
    線分分割しつつ、回転を繰り返す  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/KochCurve.h) 
    movie: なし

* コッホ雪片  
    三角形にコッホ曲線を適用すると...？  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/KochSnowflake.h) 
    movie: なし

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

* Ward  
    WardのBRDFをSRPで実装,GetWardBRDFの部分  
    Impl: [link](https://github.com/terraritto/TerakoyaRP/blob/main/Assets/ShaderLibrary/BRDF.hlsl)
    movie: [link](https://youtu.be/QhytVEdFidU)

* Mobile for URP   
    モバイル用のBRDFで使われてるやつ、URPに採用されてるのを書いただけ,GetBRDFの部分から  
    Impl: [link](https://github.com/terraritto/TerakoyaRP/blob/main/Assets/ShaderLibrary/BRDF.hlsl)
    movie: [link](https://youtu.be/4-BdB3r9Ayg)

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

* Sobel Filter  
    エッジ検出フィルタ、中心に近いほど重みが大きい  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SobelFilter.h)
    movie: [link](https://youtu.be/rXzaPR2FDGY)

* Kuwahara Filter  
    油絵っぽいものを作るフィルタ  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/KuwaharaFilter.h)
    movie:[link](https://youtu.be/po2Lr4QWV3E)

* SNN Filter  
    絵画調フィルタ、対称にピクセル比較を行っていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SNNFilter.h)
    movie:[link](https://youtu.be/39c-2kxq8YE)

* Bayar Filter  
    カメラで使われているベイヤーフィルター  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BayarFilter.h)
    movie: [link](https://youtu.be/JyxDIZewrMI)

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

* Quad
    矩形の交差、平面に近いもの  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/QuadTrace.h)
    movie: [link](https://youtu.be/aMzaBILTPk4)

* Disk
    円の交差、平面をうまく使ってく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/DiskTrace.h)
    movie: [link](https://youtu.be/deryOZpNaec)

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

* glTF Loader
    Tinygltfを使って色々読み込みしてみた  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Renderer/Loader/GLTFLoader.cpp)
    movie: [link](https://youtu.be/B_O0Y2OnAtM)

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

* Walker's Alias法  
    重み付きのサンプルをO(1)でやる  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Sampling/WalkerAlias.h)
    movie: [link](https://youtu.be/lYdXCFzLeZ8)

## 数値計算
* ニュートン法  
    ニュートン法で簡単な式を解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/NewtonSample.h) 
    movie: [link](https://youtu.be/2eWT7Tvc_TA)

* 前進オイラー法  
    前進オイラー法でdx/dt=xを解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/ForwardEulerMethod.h) 
    movie: [link](https://youtu.be/LWEyAsr9sZ4)

* ルンゲクッタ(4段)  
    ルンゲクッタの実装、単振り子を解いてます.コードには2段と3段もあり.  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/RKSample.h) 
    movie: [link](https://youtu.be/yp_wBw-Xlac)

* ランダムウォーク(2D)  
    格子上のランダムウォーク  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/RandomWork2D.h) 
    movie: [link](https://youtu.be/e5vXMGqfKyM)

* 二重振り子  
    ルンゲクッタで二重振り子を解いて可視化  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/DoublePendulum.h) 
    movie: [link](https://youtu.be/6imLz_0Z0JQ)

## 分子動力学シミュレーション
* Lennard-Jones potential  
    分子間の力を可視化してみようの会  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/LennardJonesSample.h) 
    movie: [link](https://youtu.be/_mZgFhwCjI8)

* Maxwell-Boltzmann分布  
    速度の分布を可視化してみようの会  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/MaxwellBoltzmann%20.h) 
    movie: [link](https://youtu.be/zjPBheeGWSU)

* 速度ベルレ法  
    粒子の移動計算を解く際の便利な方法  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/MolecularDynamics.h) 
    movie: [link](https://youtu.be/xqL4QVXaBEs)

## CV
* Canny Edge Detection
    Canny法の実装、OpenCVを使わずに実装してみる  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/CannyEdgeDetector.h) 
    movie: [link](https://youtu.be/38h_y5aEnK4)

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

* ベルマンフォード法  
    負もいける最短経路探索  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/ShortestPath/BellmanFordSample.h)
    movie: [link](https://youtu.be/Qn4Teb4eSpY)  

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

## 強化学習
* Greedy法  
    貪欲にバンディットしていこう！  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BanditGreedy.h) 
    movie: [link](https://youtu.be/NuEzgazAwPY)

* ε-Greedy法  
    たまには別のものも選ぼう  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BunditEpsilonGreedy.h) 
    movie: [link](https://youtu.be/KZ6SZULXBxM)

* 楽観的初期値  
    初期値は寛容にするとよいらしい  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/OptimisticInitial.h) 
    movie: [link](https://youtu.be/toW90NjTVsQ)

* 上限信頼区間(UCB)  
    ステップ数をうまくlogオーダーで組み込む  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/UCB.h) 
    movie: [link](https://youtu.be/unzkWQ8fZo8)

* 勾配バンディットアルゴリズム  
    ソフトマックスを使ってうまく学習をしていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/GradientBaseline.h) 
    movie: [link](https://youtu.be/BvYcs_WCL9c)

* ベルマン方程式  
    ベルマン方程式を使ってグリッドワールドを解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_03/GridWorld.h) 
    movie: [link](https://youtu.be/5HTBPTLxkCQ)

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
