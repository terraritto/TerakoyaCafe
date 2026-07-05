### Intersection
* Sphere  
    球の交差  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsSphere.hlsl)
    movie: [link](https://youtu.be/5d0wgHcvQfA)
    memo: [link]()

* Plane
    平面の交差  
    Impl: [link](https://github.com/terraritto/DXLab/blob/main/Shader/DXR/Intersection/chsPlane.hlsl)
    movie: [link](https://youtu.be/lA3_zsVZJ-M)
    memo: [link](../../Memo/CG/Intersection/Plane.md)

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