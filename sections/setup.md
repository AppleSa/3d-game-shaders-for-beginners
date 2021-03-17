[:arrow_backward:](gamma-correction.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](building-the-demo.md)

# 3D 游戏着色器初学

## 设置

<p align="center">
<img src="https://i.imgur.com/fYpIWNk.gif" alt="设置" title="设置">
</p>

以下是用于开发和测试示例代码的设置。

### 环境

示例代码是使用以下环境开发和测试的。

- Linux manjaro 4.9.135-1-MANJARO
- OpenGL renderer string: GeForce GTX 970/PCIe/SSE2
- OpenGL version string: 4.6.0 NVIDIA 410.73
- g++ (GCC) 8.2.1 20180831
- Panda3D 1.10.1-1

### 材质

用于构建 `mill-scene.egg` 的每种 [Blender](https://blender.org) 材料都具有按照下面顺序的五种纹理

- Diffuse [扩散]
- Normal  [默认]
- Specular [高光]
- Reflection [反射]
- Refraction [折射]

所有模型的相同位置都有相同的贴图，
着色器可以通用化，减少了重复代码的需要。

如果一个对象使用其顶点法线，则使用“浅蓝色”法线贴图。

<p align="center">
<img src="https://i.imgur.com/tFmKgoH.png" alt="平面法线贴图" title="平面法线贴图">
</p>

这是平面法线贴图的示例。
它包含的唯一颜色是纯蓝色 `(red = 128, green = 128, blue = 255)`.
这个颜色代表了一个单位（长度为 1）在正z轴通常的指向 `(0, 0, 1)`.

```c
(0, 0, 1) =
  ( round((0 * 0.5 + 0.5) * 255)
  , round((0 * 0.5 + 0.5) * 255)
  , round((1 * 0.5 + 0.5) * 255)
  ) =
    (128, 128, 255) =
      ( round(128 / 255 * 2 - 1)
      , round(128 / 255 * 2 - 1)
      , round(255 / 255 * 2 - 1)
      ) =
        (0, 0, 1)
```

这里的看到的单位 `(0, 0, 1)`
转换为RGB的浅蓝色 `(128, 128, 255)`
并将浅蓝色转换为单位法线。
你将通过 [法线贴图](normal-mapping.md) 进一步了解这一点

<p align="center">
<img src="https://i.imgur.com/R9FgZKx.png" alt="镜面贴图" title="镜面贴图">
</p>

Up above is one of the specular maps used.
The red and blue channel work to control the amount of specular reflection seen based on the camera angle.
The green channel controls the shininess factor.
You'll learn more about this in the [lighting](lighting.md) and [fresnel factor](fresnel-factor.md) sections.

The reflection and refraction textures mask off the objects that are either reflective, refractive, or both.
For the reflection texture, the red channel controls the amount of reflection and the green channel controls how
clear or blurry the reflection is.

### Panda3D

The example code uses
[Panda3D](https://www.panda3d.org/)
as the glue between the shaders.
This has no real influence over the techniques below,
meaning you'll be able to take what you learn here and apply it to your stack or game engine of choice.
Panda3D does provide some conveniences.
I have pointed these out so you can either find an equivalent convenience provided by your stack or
replicate it yourself, if your stack doesn't provide something equivalent.

Three Panda3D configurations were changed for the purposes of the demo program.
You can find these in [config.prc](../demonstration/config.prc).
The configurations changed were
`gl-coordinate-system default`,
`textures-power-2 down`, and
`textures-auto-power-2 1`.
Refer to the
[Panda3D configuration](http://www.panda3d.org/manual/?title=Configuring_Panda3D)
page in the manual for more details.

Panda3D defaults to a z-up, right-handed coordinate system while OpenGL uses a y-up, right-handed system.
`gl-coordinate-system default` keeps you from having to translate between the two inside your shaders.
`textures-auto-power-2 1` allows us to use texture sizes that are not a power of two if the system supports it.
This comes in handy when doing SSAO and other screen/window sized related techniques since the screen/window size
is usually not a power of two.
`textures-power-2 down` downsizes our textures to a power of two if the system only supports texture sizes being a power of two.

## Copyright

(C) 2019 David Lettier
<br>
[lettier.com](https://www.lettier.com)

[:arrow_backward:](gamma-correction.md)
[:arrow_double_up:](../README.md)
[:arrow_up_small:](#)
[:arrow_down_small:](#copyright)
[:arrow_forward:](building-the-demo.md)
