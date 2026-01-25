# 実装したいリスト
## 色空間
* CIE XYZ色空間
* Luv色空間
* LogLuv
* sRGB色空間
* ガンマ補正
* YCrRb
* YCbCr
* YIQ
* S-Video(YC)

## UV関係
* Texture mapping
* perspective correct
* cube texture mapping
* tangent space
* bump mapping
* normal mapping
* displacement mapping
* environment mapping
* parallax mapping
* horizontal mapping
* AO mapping
* irradiance environment mapping

## Textureの補間
* Bilinear補間
* Bicubic補間

## Raster base
* scissor rect
* depth test(階層化あり)
* stencil test
* depth bounds test
* ~~グローシェーディング~~
* ~~フォンシェーディング~~
* スキャンライン法
* warnockのアルゴリズム

## Projection
* 全部の描画を行う projection matrix
* ortho graphic projection
* oblique clipping planes

## shadow
* Cascade Shadow Mapping
* Shadow depth offset
* stencil shadows
* atmospheric shadowing
* Deep Shadow Map

## culling
* polygon clliping
* polyhedron clliping
* frustum culling
* light culling
* shadow culling
* 隠線消去

## technique
* decal
* billboard
* Halo
* motion blur
* marching cube
* 環境マップの2D LUT作成
* Sunlight and skylight
* lucas-kanade法
* depth from stereo
* light field
* IBL
* 環境マップによるIBL
* pre filtering
* BTF

## ネタ
* 円を描く
* Spline teapot
* 正四面体の描画
* Flood Fill Algorithm
* convex hull

## AO
* HBAO

## BRDF
* Lafortune
* He
* Schlick近似
* Schlickモデル
* Ashikhmin&Shirley
* Oren-Nayar

## fractal
* シェルピンスキの三角形
* pentagonal gasket
* fractal swiss flag
* ~~koch snowflake~~
* C-curve
* fractal hangman
* fractal bush
* fractal tree
* fractal star
* fractal leaf
* hilbert curve
* fractal staircase
* カントール集合
* fractal pentagram
* fractal diamond
* fractal tower
* Ron's algorithm
* Tao's algorithm
* マンデルブロ集合

## 数値計算
* fixed-point root finding algorithm
* ガウスの消去法
* ヤコビ法
* ガウス・ザイデル法
* 離散フーリエ変換
* 逆離散フーリエ変換
* FFT
* クロマサンプリング
* 離散コサイン変換(DCT)
* ビュフォンの針
* 台形公式
* Southwell緩和法
* Chebyshev iteration
* conjugate gradient method
* Binary Search
* Regular falsi
* ラゲール法
* スツルム列
* デカルトの符号則
* BFGS法
* Lagrange multiplier
* Chi-Square Distribution
* Students-T Distribution
* k-means
* SVD/PCA
* wavelet変換

## JPEG
* 量子化テーブル
* 量子化テーブルのスケーリング
* ハフマン符号化
* ランレングス符号化

## Boids
* boids
* 近傍探索
* 当たり判定

## ブロッブ解析
* 周辺画素参照法
* 輪郭追跡法
* シードフィル
* ランレングスコード法
* ランレングスコード符号化

## RayTracing手法
* adaptive supersampling
* statistical supersampling
* thin lens
* motion blur
* DOF
* Whitted Method
* Heckbert Method
* 分散レイトレーシング
* 拡散レイトレーシング
* Light Tracing
* BDPT
* MLT
* Photon mapping
* Photon輸送シミュレーション
* カーネル密度推定
* 最近傍推定
* AMR法
* Final Gathering
* Beam Tracing
* irradiance caching
* irradiance gradient
* Raycasting法
* RayMarching
* Adaptive RayMarching
* PRT
* All-Frequency PRT

## RayTracing Texture
* Stripe Texture
* Perlin Texture
* Marble Texture

## Raytracing Mapping
* Sphere Mapping
* 凸四角形
* 円
* Cylinder
* Cone

## Volume
* Volume Rendering
* Henyey-Greestein
* Rayleigh
* PhotonmappingでのVolume Rendering

## SSS
* ダイポールモデル
* nonconstant media
* Single Scattering
* multi Scattering
* TranslucentMaterial
* マルチポールモデル
* マルチレイヤーモデル

## Tonemap
* tumblin rushmeier tonmap
* ward tonemap
* ward's histogram method
* 対数平均輝度
* 輝度スケーリング演算子
* Reinhard Tone Mapping
* Extended Reinhard Tone Mapping

## サンプリング
* importance Sampling
* 層化サンプリング
* N-Rooks Sampling
* MIS
* QMC
* RIS
* Jittered
* Multi-Jitterd
* Poisson disk

## QMC列
* Hammersley
* Halton
* Sobol
* Niederreither
* van der corput

## ライト選択
* 通常ライト選択
* 面積によるライト選択
* 光量によるライト選択
* light cuts
* multidimensional lightcuts

## dither
* random dither
* ordered dither
* error diffusion dither

## filter
* box filer
* tent filter
* cubic spline filter
* sinc
* lanczos
* low pass filter
* notch filter
* comb filter
* weighted reconstruction filter

## レイトレ交差
* Cylinder
* Cone
* Torus
* BOX
* general quadric surfrace
* Super Quadrics
* Metaball
* surface of revolution
* ベジェ曲線
* ベジェ曲面
* B-spline曲線
* B-spline局面
* 3DLine
* NURBUS Surface

## Spline
* de Casteljauのアルゴリズム
* バーンステイン基底関数
* B-Spline
* NURBS
* de Casteljauによる曲面
* バーンステイン基底関数による曲面
* de Casteljauの再分割アルゴリズム
* Boehm's knot insertion algorithm
* Oslo Algorithm
* Lane-Riesenfield Algorithm
* Schaefer's knot insertion algorithm
* box splineの細分割
* quadrilateral meshの細分割
* triangular meshの細分割
* Bicubic Patch

## ラジオシティ
* ラジオシティ法
* Gauss-Seidel Radiosity
* Progressive Radiosity
* Bidirectional Radiosity
* Hemicube Algorithm
* Random walkのRadiosity
* ヒストグラム法
* Instant Radiosity

## 構造
* CSG
* B-Rep
* Winged-edge

## AS
* Octree
* BVH
* BSP
* KD-Tree
* regular grid

## 乱数
* LCG

## PostProcess
* fullscreen pass(三角形で全描画)
* gameboy風
* アスキーアート風

## 探索
* ~~重みつきA*~~
* anytime repairing A*
* 山登り法(HC)
* 強制山登り法(EHC)
* 焼きなまし法(SA)
* 二分ヒープ
* フィボナッチヒープ
* 基数ヒープ
* バケットプライオリティキュー
* 連鎖法(ハッシュ解決)
* 開番地法(ハッシュ解決)
* 置き換え法(ハッシュ解決)
* 剰余法(ハッシュ関数)
* 積算ハッシュ法(ハッシュ関数)
* ゾブリストハッシュ法(ハッシュ関数)
* 差分ハッシュ(ハッシュ関数)
* 遅延重複検出(ハッシュ)
* 反復深化深さ優先探索(DFID)
* コスト制限付き深さ優先探索(CLDFS)
* 反復深化A*(IDA*)
* コスト制限付き深さ優先探索A*(CLDFS-IDA*)
* 両方向探索
* meet in the middle法
* perimeter search
* シンボリック幅優先木探索
* シンボリック幅優先グラフ探索
* シンボリックA*探索
* ビームサーチ
* 単調ビームサーチ
* Top-kサンプリング
* Top-pサンプリング
* モンテカルロ木探索
* 深さ優先分枝限定法(DFBnB)
* 幅制限探索(WBS)
* 反復幅制限探索
* 最良優先幅制限探索(BFWS)
* 並列A*探索(PA*)
* ハッシュ分配A*探索(HDA*)
* 簡約ゾルリストハッシュ法(AZH)
* PRA*
* TDS
* 外部記憶幅優先探索(External BrFS)
* 外部記憶A*探索(external A*)
* パターンデータヒューリスティック
* Structured Duplicate Detection
* Batch A*
* Expectiminimax法