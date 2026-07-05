# Plane  
平面との交差を考えていく.  
今回やるのは無限平面で、範囲制限はない.  
まず最初に平面を点$`\vec{a}`$と法線$`\vec{n}`$とする.  
また、Rayの交差する点を$`\vec{r}`$とすると、法線と平面と平行なベクトル$`\vec{r}-\vec{a}`$は垂直なので0になるはず.  
```math
\begin{equation}
    \begin{split}
    (\vec{r} - \vec{a}) \cdot \vec{n} = 0
    \end{split}
\end{equation}
```
さて、光線は$`\vec{r} = \vec{o} +t \vec{d}`$だった.  
なのでこれを入れると、  
```math
\begin{equation}
    \begin{split}
    & (\vec{r} - \vec{a}) \cdot \vec{n} = 0 \\
    & (\vec{o} +t \vec{d} - \vec{a}) \cdot \vec{n} = 0 \\
    & (t\vec{d}) \cdot \vec{n} = (\vec{a} - \vec{o}) \cdot \vec{n} \\
    & t = \frac{(\vec{a} - \vec{o}) \cdot \vec{n}}{\vec{d} \cdot \vec{n}}
    \end{split}
\end{equation}
```
あとはこれを式に落とし込めばOK.  
hlslで実装したものが以下である.  
```c++
bool IntersectToPlane(float3 center, float3 normal, out float thit) 
{
    float t = dot((center - ObjectRayOrigin()), normal) / dot(ObjectRayDirection(), normal);

    if (t > EPSILON)
    {
        thit = t;
        return true;
    }

    return false;
}
```
結果は以下のような感じ.  
動画のしか残ってなかったので、文字があるけど許してね.  
![Plane](Image/Plane.webp)