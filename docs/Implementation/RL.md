# 強化学習周りの実装  
* Greedy法  
    貪欲にバンディットしていこう！  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BanditGreedy.h) 
    movie: [link](https://youtu.be/NuEzgazAwPY)
    memo: [link](../Memo/RL/Sutton/Ex02/BunditGreedy.md)

* ε-Greedy法  
    たまには別のものも選ぼう  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/BunditEpsilonGreedy.h) 
    movie: [link](https://youtu.be/KZ6SZULXBxM)
    memo: [link](../Memo/RL/Sutton/Ex02/BunditEpsilonGreedy.md)

* 楽観的初期値  
    初期値は寛容にするとよいらしい  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/OptimisticInitial.h) 
    movie: [link](https://youtu.be/toW90NjTVsQ)
    memo: [link](../Memo/RL/Sutton/Ex02/OptimisticInitial.md)

* 上限信頼区間(UCB)  
    ステップ数をうまくlogオーダーで組み込む  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/UCB.h) 
    movie: [link](https://youtu.be/unzkWQ8fZo8)
    memo: [link](../Memo/RL/Sutton/Ex02/UCB.md)

* 勾配バンディットアルゴリズム  
    ソフトマックスを使ってうまく学習をしていく  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/GradientBaseline.h) 
    movie: [link](https://youtu.be/BvYcs_WCL9c)
    memo: [link](../Memo/RL/Sutton/Ex02/GradientBaseline.md)

* ベルマン方程式  
    ベルマン方程式を使ってグリッドワールドを解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_03/GridWorld.h) 
    movie: [link](https://youtu.be/5HTBPTLxkCQ)
    memo: [link](../Memo/RL/Sutton/Ex03/BellmanEquation.md)

* ベルマン最適方程式  
    ベルマン最適方程式でグリッドワールドの最大化を可視化  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_03/GridWorldNext.h) 
    movie: [link](https://youtu.be/FvRFekLwlz4)
    memo: [link](../Memo/RL/Sutton/Ex03/BellmanOptimalityEquation.md)

* 動的計画法(DP)  
    DPによるグリッドワールドの計算  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_04/04_01_GridWorld.h) 
    memo: [link](../Memo/RL/Sutton/Ex04/PolicyEvaluation.md)

* Jack's Car Rental Probrem  
    ジャックのカーレンタル問題を解く  
    Impl: [link](https://github.com/terraritto/Siv3DImplementZoo/blob/main/RL/Exp_04/04_02_JacksCarRental.h) 
    memo: [link](../Memo/RL/Sutton/Ex04/JacksCarRentalProblem.md)