# 材质

### 知识回顾
在纹理这一课中，我们学习了关于纹理贴图如何使用的方法

>* 将纹理坐标的信息存储到VAO中

![纹理坐标](图片/纹理坐标.png)

如此，正方形的4个顶点分别对应着

![纹理坐标解释](图片/纹理坐标解释.png)

>* 在像素着色器中使用 texture 函数采样

![采样函数](图片/采样函数.png)

如此，便将一张图片附着在正方形上。

### 材质定义
以对光的描述来定义材质，这也是冯氏光照模型的特点。

![材质定义](图片/材质定义.png)

其中

>* **ambient**   : 环境光，是一个非常的小的亮度，单纯的环境光的效果相当于，在昏暗的环境中的大致颜色。
>* **diffuse**   : 漫反射，基础色，主要表现的颜色。
>* **specular**  : 高光点。
>* **shininess** : 高光参数。

所谓材质，即这一套在实际上的表现。

### 参数调整
首先我们在 [shadertoy](https://www.shadertoy.com/) 上看看用教程上的例子的效果:

>* 光照参数

``` C++
// the light input

mat3 PhongLight = mat3( 0.2,  0.2,  0.2,  // ambient
                    0.8,  0.8, 0.8,      // diffuse
                    1.0, 1.0, 1.0 );    // specular
#define P_ambient  0
#define P_diffuse  1
#define P_specular 2
    
void mainImage( out vec4 fragColor, in vec2 fragCoord )
{
    // Normalized pixel coordinates (from 0 to 1)
    vec2 uv = fragCoord/iResolution.xy;

    // Time varying pixel color
    vec3 col = 0.5 + 0.5*cos(iTime+uv.xyx+vec3(0,2,4));
    
    col = PhongLight[P_specular];
    
    // Output to screen
    fragColor = vec4(col,1.0);
}

```


