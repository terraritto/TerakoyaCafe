# メモまとめ
* CG  
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

    * Mesh Shader Series
        - 001 ポリゴン表示: [link](CG/MeshShaderSeries/001_Intro.md) 
        [Impl](https://github.com/terraritto/MeshShaderPractice/tree/main/Sample_001) 
        [movie](https://youtu.be/fhdpNOXFf3I)

        - 002 モデル描画準備: [link](CG/MeshShaderSeries/002_DrawMesh.md)  

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

* Sutton Reinforcement Learning
    * 2章
        - Bundit Greedy: [link](RL/Sutton/Ex02/BunditGreedy.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BanditGreedy.h) 
        [movie](https://youtu.be/NuEzgazAwPY)

        - Bundit Epsilon Greedy: [link](RL/Sutton/Ex02/BunditEpsilonGreedy.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BunditEpsilonGreedy.h) 
        [movie](https://youtu.be/KZ6SZULXBxM)

        - Optimistic Initial: [link](RL/Sutton/Ex02/OptimisticInitial.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/OptimisticInitial.h) 
        [movie](https://youtu.be/toW90NjTVsQ)

        - UCB: [link](RL/Sutton/Ex02/UCB.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/UCB.h) 
        [movie](https://youtu.be/unzkWQ8fZo8)

        - Gradient Baseline: [link](RL/Sutton/Ex02/GradientBaseline.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/GradientBaseline.h) 
        [movie](https://youtu.be/BvYcs_WCL9c)
    
    * 3章
        - Bellman Equation: [link](RL/Sutton/Ex03/BellmanEquation.md) 
        [impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_03/GridWorld.h) 
        [movie](https://youtu.be/5HTBPTLxkCQ)

        - Bellman Optimality Equation: [link](RL/Sutton/Ex03/BellmanOptimalityEquation.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_03/GridWorldNext.h) 
        [movie](https://youtu.be/FvRFekLwlz4)   

    * 4章
        - Policy Evaluation: [link](RL/Sutton/Ex04/PolicyEvaluation.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_04/04_01_GridWorld.h) 

        - Jacks Car Rental Problem: [link](RL/Sutton/Ex04/JacksCarRentalProblem.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_04/04_02_JacksCarRental.h) 

        - Gamblers Problem: [link](RL/Sutton/Ex04/GamblerProblem.md) 
        [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_04/04_03_GamblersProblem.h)

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

* TIPS
    - 同名ファイルの出力におけるObj衝突の解消: [link](TIPS/Obj_dump.md)
    
    - DefaultTextureを作るためのツール: [link](TIPS/DefaultTexture.md) 
    [Impl](https://github.com/terraritto/Siv3DImplementZoo/blob/main/Zoo/DefaultTextureCreator.h)