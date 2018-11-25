# ����

### ֪ʶ�ع�
��������һ���У�����ѧϰ�˹���������ͼ���ʹ�õķ���

>* �������������Ϣ�洢��VAO��

![��������](ͼƬ/��������.png)

��ˣ������ε�4������ֱ��Ӧ��

![�����������](ͼƬ/�����������.png)

>* ��������ɫ����ʹ�� texture ��������

![��������](ͼƬ/��������.png)

��ˣ��㽫һ��ͼƬ�������������ϡ�

### ���ʶ���
�ԶԹ��������������ʣ���Ҳ�Ƿ��Ϲ���ģ�͵��ص㡣

![���ʶ���](ͼƬ/���ʶ���.png)

����

>* **ambient**   : �����⣬��һ���ǳ���С�����ȣ������Ļ������Ч���൱�ڣ��ڻ谵�Ļ����еĴ�����ɫ��
>* **diffuse**   : �����䣬����ɫ����Ҫ���ֵ���ɫ��
>* **specular**  : �߹�㡣
>* **shininess** : �߹������

��ν���ʣ�����һ����ʵ���ϵı��֡�

### ��������
���������� [shadertoy](https://www.shadertoy.com/) �Ͽ����ý̳��ϵ����ӵ�Ч��:

>* ���ղ���

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


