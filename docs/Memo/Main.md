# メモまとめ
* CG  
    * Basics
        - ブレゼンハムのアルゴリズム: [link](CG/Basics/Bresenham.md) 
            [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/BresenhamSample.h) 
            [Movie](https://youtu.be/mRRQs_vFNXk)

        - DDA: [link](CG/Basics/DDA.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/DDASample.h)  

        - Cohen-Sutherlandのアルゴリズム: [link](CG/Basics/CohenSutherland.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/CohenSutherlandSample.h) 
        [Movie](https://youtu.be/g4Ix0TcBJ-Q)

        - Cyrus-Beckのアルゴリズム: [link](CG/Basics/CyrusBeck.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/CyrusBeckSample.h) 
        [Movie](https://youtu.be/DpWlDtt9D2s)

        - 三角形の塗りつぶし: [link](CG/Basics/Rasterization_01.md)
        [Movie](https://youtu.be/uHUEoTq2w10)  
            - Triangle: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/TriangleEdgeFunctionSample.h)  
            - 多角形: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/PentagonEdgeFunctionSample.h)  

        - 3D座標変換: [link](CG/Basics/Trans.md) 
        [movie](https://youtu.be/Ut00Yn21Ohk)
            - 実際の実装: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/VertexProcess.h)  
            - 基底の可視化: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/BasisSample.h)
            - NDCのグラフ: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/NDCPlot.h)

    * フィルタ
        - べイヤーフィルタ: [link](CG/Filter/BayarFilter.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BayarFilter.h) 
        [Movie](https://youtu.be/JyxDIZewrMI)

        - バイラテラルフィルタ: [link](CG/Filter/BilateralFilter.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/BilateralFilter.h) 
        [movie](https://youtu.be/WwVQfapRkXw)

        - Kuwaharaフィルタ: [link](CG/Filter/KuwaharaFilter.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/KuwaharaFilter.h) 
        [movie](https://youtu.be/po2Lr4QWV3E)

        - 平均値フィルタ: [link](CG/Filter/MeanFilter.md) 
        [Impl](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl) 
        [movie](https://youtu.be/zm-NbJyQamg)

        - 1パスガウシアンフィルタ: [link](CG/Filter/OnePassGaussianFilter.md) 
        [Impl](https://github.com/terraritto/DXLab/blob/main/Shader/PostProcess/CommonBlurPS.hlsl) 
        [movie](https://youtu.be/zm-NbJyQamg)

        - SNNフィルタ: [link](CG/Filter/SNNFilter.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SNNFilter.h) 
        [movie](https://youtu.be/39c-2kxq8YE)

        - ソーベルフィルタ: [link](CG/Filter/SobelFilter.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CV/SobelFilter.h) 
        [movie](https://youtu.be/rXzaPR2FDGY)
    
    *  フラクタル
        - カントール集合1D: [link](CG/Fractal/Contor1D.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Cantor1D.h) 

        - カントール集合2D: [link](CG/Fractal/Contor2D.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Cantor2D.h) 
        
        - カントール集合3D: [link](CG/Fractal/Contor3D.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Cantor3D.h) 

        - コッホ曲線: [link](CG/Fractal/KochCurve.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/KochCurve.h) 
        [movie](https://youtu.be/yY6GhZ8SMX4)

        - コッホ雪片: [link](CG/Fractal/KochSnowfrake.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/KochSnowflake.h) 
        [movie](https://youtu.be/yY6GhZ8SMX4)

        - シェルピンスキーのギャスケット: [link](CG/Fractal/SierpinskiGasket.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Sierpinski.h)

        - メンガーのスポンジ: [link](CG/Fractal/MengerSponge.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Menger.h)

        - マンデルブロ集合: [link](CG/Fractal/Mandelbrot.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Mandelbrot.h)

        - ジュリア集合: [link](CG/Fractal/Julia.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/Julia.h)

        - バーニング・シップ: [link](CG/Fractal/BurningShip.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/BurningShip.h)

        - バーンズリーのシダ: [link](CG/Fractal/BernsleyFern.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/BernsleyFern.h)

        - ペアノ曲線: [link](CG/Fractal/PeanoCurve.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/PeanoCurve.h)

        - ヒルベルト曲線: [link](CG/Fractal/HilbertCurve.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/HilbertCurve.h)

        - 高木曲線: [link](CG/Fractal/TakagiCurve.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/TakagiCurve.h)

        - リアプノフ・フラクタル: [link](CG/Fractal/Lyapunov.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Fractal/lyapunov.h)
    
    * レイトレ交差判定
        - Plane: [link](CG/Intersection/Plane.md) 
        [Impl](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsPlane.hlsl) 
        [movie](https://youtu.be/lA3_zsVZJ-M)

        - Sphere: [link](CG/Intersection/Sphere.md) 
        [Impl](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsSphere.hlsl)
        [movie](https://youtu.be/5d0wgHcvQfA)

        - Quad: [link](CG/Intersection/Quad.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/QuadTrace.h) 
        [movie](https://youtu.be/aMzaBILTPk4)

        - Disk: [link](CG/Intersection/Disk.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/DiskTrace.h) 
        [movie](https://youtu.be/deryOZpNaec)

    * 色
        - Brutonのアルゴリズム: [link](CG/Color/BrutonWavelength.md) 
        [Impl](https://github.com/terraritto/ColorForSiv3D/blob/main/Graph/BrutonWaveLength.h)

        - 三色説と分光感度グラフ: [link](CG/Color/SmithPokorny.md) 
        [Impl](https://github.com/terraritto/ColorForSiv3D/blob/main/Graph/SmithPokornyGraph.h)

        - 反対色説から段階説へ: [link](CG/Color/ColorOpponent.md) 
        [Impl](https://github.com/terraritto/ColorForSiv3D/blob/main/Graph/PrimaryColorToOpponentColor.h)

        - 分光視感効率について: [link](CG/Color/LuminousEfficiency.md) 
        [Impl](https://github.com/terraritto/ColorForSiv3D/blob/main/Graph/LuminousEffectivity.h)

        - CIE1931のRGB値を描画してみる: [link](CG/Color/CIE1931RGB.md) 
        [Impl](https://github.com/terraritto/ColorForSiv3D/blob/main/CIE1931/CIE1931RGBGraph.h)

    * Paper
        - Lumoを実装してみる: [link](CG/Paper/Lumo.md) 
        [Impl](https://github.com/terraritto/RecalculateNormalNPR/tree/main/Lumo)

    * Mesh Shader Series
        - 001 ポリゴン表示: [link](CG/MeshShaderSeries/001_Intro.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/tree/main/Sample_001) 
        [movie](https://youtu.be/fhdpNOXFf3I)

        - 002 モデル描画準備: [link](CG/MeshShaderSeries/002_DrawMesh.md)   

        - 003 Meshletによる描画 API編: [link](CG/MeshShaderSeries/003_SimpleMeshlet.md) [movie](https://youtu.be/9i3mjXuW_NU)  
            DirectX Mesh/MeshOptimizer利用の実装: [Impl](https://github.com/terraritto/MeshShaderPractice/blob/main/Base/Graphics/Resource/ResourceModel.cpp)  

        - 004 Meshletによる描画 Greedy編: [link](CG/MeshShaderSeries/004_MeshletGreedy.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/blob/main/Base/Graphics/Resource/ResourceModel.cpp) 
        [movie](https://youtu.be/b8cRv2MZgI0)

        - 005 Mesh ShaderでMeshletによる描画 Bounding Sphere編: [link](CG/MeshShaderSeries/005_MeshletBoundingSphere.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/blob/main/Base/Graphics/Resource/ResourceModel.cpp) 

        - 006 Instancingで沢山出してみよう！ ～ ASを添えて ～: [link](CG/MeshShaderSeries/006_Instancing.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/tree/main/Sample_003)
        - 006 Appendix 01 ImGuiの初期化の書き方が変わってた件: [link](CG/MeshShaderSeries/Appendix_006_01_Imgui.md)  
        - 006 Appendix 02 PIPELINE_STATISTICSについて: [link](CG/MeshShaderSeries/Appendix_006_02_Statistics.md)  
        - 007 Mesh Shaderによるカリング ～ Meshlet編1 FrustumCulling ～: [link](CG/MeshShaderSeries/007_MeshletCulling.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/tree/main/Sample_004)

* Math
    * Pi
        - ウォリスの公式: [link](Math/Pi/WallisProduct.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/WallisSample.h)

        - グレゴリー級数とシャープの方法: [link](Math/Pi/GregorySharp.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Numerical/GregorySharpSample.h)  

* PseudoRand
    - 線形合同法: [link](PseudoRand/LCG.md)  
        - RANDU: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Pseudo/RANDUSample.h)
        - MINSTD: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Pseudo/LehmerRandomSample.h)
        - LCG: [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/CG/Pseudo/LCGSample.h)

* Algorithm
    * Shuffle
        - フィッシャー–イェーツのシャッフル: [link](Algorithm/Shuffle/FisherYates.md) 
        [Impl](https://github.com/terraritto/ShuffleAlgo/blob/main/FisherYates.h)

* ODS
    * 2章
        - Array: [link](ODS/Ex02/001_Array.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/TArray.h) 

        - ArrayStack: [link](ODS/Ex02/002_ArrayStack.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayStack.h) 

        - FastArrayStack: [link](ODS/Ex02/003_FastArrayStack.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/FastArrayStack.h) 

        - ArrayQueue: [link](ODS/Ex02/004_ArrayQueue.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayQueue.h) 

        - ArrayDeque: [link](ODS/Ex02/005_ArrayDeque.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/ArrayDeque.h) 

        - DualArrayDeque: [link](ODS/Ex02/006_DualArrayDeque.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/DualArrayDeque.h) 

        - RootishArrayStack: [link](ODS/Ex02/007_RootishArrayStack.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_02/RootishArrayStack.h) 

    * 3章
        - SLList: [link](ODS/Ex03/001_SLList.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/SLList.h)

        - DLList: [link](ODS/Ex03/002_DLList.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/DLList.h)

        - BDeque: [link](ODS/Ex03/003_BDeque.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/BDeque.h) 

        - SEList: [link](ODS/Ex03/004_SEList.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/SEList.h) 

        - XORList: [link](ODS/Ex03/005_XorList.md) 
        [Impl](https://github.com/terraritto/DataStructurePractice/blob/main/Ex_03/XORList.h)

* Sutton Reinforcement Learning
    * 2章
        - Bundit Greedy: [link](RL/Sutton/Ex02/BunditGreedy.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_02/BanditGreedy.h) 
        [movie](https://youtu.be/NuEzgazAwPY)

        - Bundit Epsilon Greedy: [link](RL/Sutton/Ex02/BunditEpsilonGreedy.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_02/BunditEpsilonGreedy.h) 
        [movie](https://youtu.be/KZ6SZULXBxM)

        - Optimistic Initial: [link](RL/Sutton/Ex02/OptimisticInitial.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_02/OptimisticInitial.h) 
        [movie](https://youtu.be/toW90NjTVsQ)

        - UCB: [link](RL/Sutton/Ex02/UCB.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_02/UCB.h) 
        [movie](https://youtu.be/unzkWQ8fZo8)

        - Gradient Baseline: [link](RL/Sutton/Ex02/GradientBaseline.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_02/GradientBaseline.h) 
        [movie](https://youtu.be/BvYcs_WCL9c)
    
    * 3章
        - Bellman Equation: [link](RL/Sutton/Ex03/BellmanEquation.md) 
        [impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_03/GridWorld.h) 
        [movie](https://youtu.be/5HTBPTLxkCQ)

        - Bellman Optimality Equation: [link](RL/Sutton/Ex03/BellmanOptimalityEquation.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_03/GridWorldNext.h) 
        [movie](https://youtu.be/FvRFekLwlz4)   

    * 4章
        - Policy Evaluation: [link](RL/Sutton/Ex04/PolicyEvaluation.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_04/04_01_GridWorld.h) 

        - Jacks Car Rental Problem: [link](RL/Sutton/Ex04/JacksCarRentalProblem.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_04/04_02_JacksCarRental.h) 

        - Gamblers Problem: [link](RL/Sutton/Ex04/GamblerProblem.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_04/04_03_GamblersProblem.h)

    * 5章
        - first-visit MC: [link](RL/Sutton/Ex05/FirstVisitMC.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_05/05_01_MCOnPolicy.h)

        - モンテカルロES: [link](RL/Sutton/Ex05/MCES.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_05/05_02_MCES.h)

        - 方策オフ型学習: [link](RL/Sutton/Ex05/OffPolicy.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_05/05_03_MCOffPolicy.h)

        - 無限の分散: [link](RL/Sutton/Ex05/InfinityVariance.md) 
        [Impl](https://github.com/terraritto/SuttonRLForSiv3D/blob/main/Exp_05/05_04_InfiniteVariance.h)

* DLFS
    * 2章
        - AND Gate: [link](DLFS/02/AndGate.md) 
        [Impl](https://github.com/terraritto/DLFS/blob/main/Ex_02/AndGate.cpp)

        - NAND Gate: [link](DLFS/02/NandGate.md) 
        [Impl](https://github.com/terraritto/DLFS/blob/main/Ex_02/NandGate.cpp)

        - OR Gate: [link](DLFS/02/OrGate.md) 
        [Impl](https://github.com/terraritto/DLFS/blob/main/Ex_02/OrGate.cpp)

        - XOR Gate: [link](DLFS/02/XorGate.md) 
        [Impl](https://github.com/terraritto/DLFS/blob/main/Ex_02/XorGate.cpp)

* Electronics
    * FPGA
        - Tang Nano 9K Lチカ: [link](Electronics/FPGA/TangNano_LBlink.md) 
        [Impl](https://github.com/terraritto/TangNano9kPractice/tree/main/Led_Blink/)

* TIPS
    - 同名ファイルの出力におけるObj衝突の解消: [link](TIPS/Obj_dump.md)
    
    - DefaultTextureを作るためのツール: [link](TIPS/DefaultTexture.md) 
    [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Zoo/DefaultTextureCreator.h)

    - 魔法少女ノ魔女裁判 Switch2のフェードを再現してみる: [link](TIPS/MagicalTrialFade.md) 
    [Impl](https://github.com/terraritto/Siv3DImplementZoo/tree/main/Zoo/MagicalTrial)

* リンクだけ
    - OS Series: [Impl](https://github.com/terraritto/OSProject)
    - PRML: [Impl](https://github.com/terraritto/Siv3DImplementZoo/tree/main/PRML)
    - GameProgrammingC++ Series: [Impl](https://github.com/terraritto/Siv3DImplementZoo/tree/main/GameProgrammingC%2B%2B)