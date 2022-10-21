---
title: CSS3
date: 2022-07-19 15:47:05
summary: CSS3
tags: [CSS3]
categories: [CSS3]
---

# CSS3

## 1. 是什么

css, 即层叠样式表(Cascading Style Sheets)的简称，是一种标记语言，由浏览器解析执行用于使页面变得更美观。

Css3 是 css 的最新标准， 是向后兼容的，css1/2的特性在css3里都是可以使用的。

而css3新增了许多新特性，为开发带来了更佳的开发体验。

## 2. 选择器

![](https://s2.loli.net/2022/10/21/P7vfUQWAOuCo1Yy.png)

## 3.新样式

### border

- border-radius: 圆角边框
- box-shadow: (inset) offset-x offset-y blur-radius spread-radius color
- border-image:  使用图片绘制边框

### background

- background-clip

|    value    |           description            |
| :---------: | :------------------------------: |
| padding-box |       背景延伸至内边距外沿       |
| border-box  | 背景延伸至边框外沿（在边框下层） |
| content-box |      背景被裁剪至内容区外沿      |
|    text     |     背景被裁剪至文字的前景色     |

- background-origin

|    value    |             description             |
| :---------: | :---------------------------------: |
| padding-box | 背景图片的摆放以 padding 区域为参考 |
| border-box  | 背景图片的摆放以 border 区域为参考  |
| content-box | 背景图片的摆放以 content 区域为参考 |

### word

- word-wrap: 

|   value   |                         description                          |
| :-------: | :----------------------------------------------------------: |
|  normal   |                      使用默认的断行规则                      |
| break-all | 对于 non-CJK (CJK 指中文/日文/韩文) 文本，可在任意字符间断行 |
| keep-all  |         CJK 文本不断行。Non-CJK 文本表现同 `normal`          |

- text-overflow: 向文本应用阴影。能够规定水平阴影、垂直阴影、模糊距离，以及阴影的颜色
- text-decoration : 是一种简写属性，包含text-decoration-line、text-decoration-color、text-decoration-style、text-decoration-thickness四种属性。

|           value           |                         description                          |
| :-----------------------: | :----------------------------------------------------------: |
|   text-decoration-line    | none、underline(文本的下方有一条修饰线)、overline(文本的上方有一条修饰线)、line-through(有一条贯穿文本中间的修饰线) |
|   text-decoration-color   |                        文本修饰的颜色                        |
|   text-decoration-style   |     solid、double(双实线)、dotted、dashed、wavy(波浪线)      |
| text-decoration-thickness |                       文本修饰线的粗细                       |

### color

- rbga: rgb为颜色，a为透明度
- hsla : h为hue色相，s为saturation饱和度，l为lightness亮度，a为alpha透明度

## 4. transition 过渡

transition 是 transition-property, transition-duration, transition-timing-function 和 transition-delay的一个简写属性。

|           value            |                         description                          |
| :------------------------: | :----------------------------------------------------------: |
|    transition-property     | 应用过渡属性的名称，如果指定的是一个简写属性，那么简写属性的所有属性都将应用过渡属性。 |
|    transition-duration     |            以秒或毫秒为单位指定过渡动画所需的时间            |
| transition-timing-function | ease、ease-in、ease-out、ease-in-out、linear、step-start、step-end、steps(4, end) |
|      transition-delay      |          规定了在过渡效果开始作用之前需要等待的时间          |

## 5. transform 转换

transform: scale / translate / rotate

## 6. animation 动画

动画这个平常用的也很多，主要是做一个预设的动画。和一些页面交互的动画效果，结果和过渡应该一样，让页面不会那么生硬

animation也有很多的属性

- animation-name：动画名称
- animation-duration：动画持续时间
- animation-timing-function：动画时间函数
- animation-delay：动画延迟时间
- animation-iteration-count：动画执行次数，可以设置为一个整数，也可以设置为infinite，意思是无限循环
- animation-direction：动画执行方向
- animation-paly-state：动画播放状态
- animation-fill-mode：动画填充模式

## 7. 渐变

颜色渐变是指在两个颜色之间平稳的过渡，`css3`渐变包括

- linear-gradient：线性渐变

> background-image: linear-gradient(direction, color-stop1, color-stop2, ...);

- radial-gradient：径向渐变

> linear-gradient(0deg, red, green);

## 8. reflect 反射

```css
-webkit-box-reflect:方向[ above-上 | below-下 | right-右 | left-左 ]，偏移量，遮罩图片
```

## 9. filter 滤镜

```
filter: [ grayscale(100%)-黑白色filter | sepia(1)-褐色 | saturate(2)-饱和度 | hue-rotate(90deg)-色相旋转 | invert(1)-反色 | opacity(.5)-透明度 | brightness(.5)-亮度 | contrast(2)-对比度 | blur(3px)-模糊度 | drop-shaodw(5px 5px 5px #000)-阴影]
```

## 10. 多列布局

![image-20221021150340219.png](https://s2.loli.net/2022/10/21/hH9XoUPtNa3AqyG.png)

## 11. 盒模型定义

```css
box-sizing: [border-box(border+padding+content) | content-box(content)]
```

## 12. 媒体查询

![image-20221021150724104.png](https://s2.loli.net/2022/10/21/WYVjyl9rfRP7D3o.png)

## 13. 其他

flex布局（弹性布局）：https://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

grid布局（栅格布局）:   https://www.ruanyifeng.com/blog/2019/03/grid-layout-tutorial.html

混合模式：background-blend-mode 、mix-blend-mode
