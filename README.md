## psd 图片转svg 代码及动画效果实践

###  为什么使用svg
--------------------

&nbsp;&nbsp;&nbsp;&nbsp;因为svg是一种可缩放的矢量图形，所以在不同尺寸下拉伸下都不会影响图像质量，且我们可以通过fill,stroke任意修改svg图形的颜色。在img标签里面我们会使用png 或者jpg图片需要网络请求图片资源，而如果使用svg我们可以减少http请求。并且svg 与jpeg和gif图像比起来，尺寸更小，并且可压缩性更强。svg 的相关教程可以在网上搜索到，但是一般我们自己用svg去写一个图形或是写一个曲线是比较麻烦的，但是我们可以使用  `Illustrator 软件` 帮助我们将psd 图片直接导出为svg代码

###  通过psd图片转换为svg路径，并实现动态效果
----------------------------

&nbsp;&nbsp;&nbsp;&nbsp;假设我们需要实现下面的这张图片的效果，从第一个图标需要射出一个曲线到第三个图标，我们如何快速实现这个曲线的动画效果。

![](http://gtms02.alicdn.com/tps/i2/TB1KBrFJXXXXXXXXpXXMZSk3XXX-221-524.png)

#### 第一步： 我们先选取到这根曲线的路径，然后`导出为ai格式`
----------------------------

![](http://gtms03.alicdn.com/tps/i3/TB1IcYGJXXXXXcFXXXXEEmwOXXX-263-281.png)

![](http://gtms04.alicdn.com/tps/i4/TB1Y9PEJXXXXXXSXpXXbzta9VXX-658-401.jpg)


#### 第二步： 通过AI软件打开ai格式的文件,调整一下颜色，然后选择文件存储为格式是svg，然后确定，就会`导出svg格式的文件`
----------------------------

#### 第三步： 打开导出的svg 脚本，把一些不要的内容删除掉
----------------------------

````
<svg version="1.1" id="图层_1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" x="0px" y="0px"
     width="612px" height="792px" viewBox="0 0 612 792" enable-background="new 0 0 612 792" xml:space="preserve">
<path fill="transparent" stroke="#189CC6" stroke-miterlimit="10" d="M212,344c0,0,20.73-139.751,96-162" stroke-width="2px"/>
</svg>

```` 
在浏览器中打开此脚本，效果如下图所示

![](http://gtms04.alicdn.com/tps/i4/TB1L3LOJXXXXXXkXXXXr0cXRpXX-513-335.png)

#### 第四步： 我们需要让这根曲线动起来，首先我们要知道在svg 中的   `g 标签`用于将多个形状形成一组，表示group,其次要想让svg 动起来，我们需要为它添加[`animateTransform`](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/animateTransform) 标签，通过在这个标签上修改transform 相关的属性，实现动画效果，与css3类似，比如：
----------------------------


````
<rect id="Rectangle-1" fill="#3C81C1" sketch:type="MSShapeGroup" x="0" y="0" width="100" height="125">
  <animateTransform attributeName="transform"
    type="translate"
    from="0 0"
    to="150 20"
    begin="0s"
    dur="2s"
    repeatCount="indefinite"
  />
</rect>
````

我们也可以针对一组group 元素去添加动画效果，比如下面：
````
<svg>
 <g>
   <g>
      .....
    </g>
   <animateTransform attributeName="transform"
            type="rotate"
            from="0 18 18"
            to="360 18 18"
            begin="0s"
            dur="0.85s"
            repeatCount="indefinite"
        />
    </g>
</svg>
````

#### 第五步：我们看上面的from to 第一个参数表示的是旋转角度从0 到 360， 第二个参数与第三个参数指的是什么呢？ 指的是旋转的中心点坐标在哪里，是相对于这组g计算的，结合上面的知识我们就可以按视觉稿作出如下动画效果，效果查看[`Jsbin在线地址`](https://jsbin.com/hahigi/edit?html,output)
----------------------------------


````
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style>
        body {
            background: #040c19;
        }
        .test {
            position: absolute;
            left: 0px;
            top: 0px;
            z-index: 2;
        }

        .m-img {
            position: absolute;
            left: 105px;
        }

        .m-img2 {
            top: 390px;
        }
    </style>
</head>
<body>
    <img src="http://g-assets.daily.taobao.net/sd/base-lab/1.0.1/img/cloud-1.png" class="m-img m-img1"/>
    <img src="http://g-assets.daily.taobao.net/sd/base-lab/1.0.1/img/cloud-1.png" class="m-img m-img2"/>
    <svg width="110px" height="512px" class="test">
        <g>
            <g transform="translate(0,118)">
                <path fill-rule="evenodd" clip-rule="evenodd" fill="#51C0DF" stroke="#51C0DF" stroke-miterlimit="10" d="M46.399,255.957
                c1.657,0,3,1.343,3,3c0,1.657-1.343,3-3,3c-1.657,0-3-1.343-3-3C43.399,257.3,44.742,255.957,46.399,255.957z"/>
                <path fill-rule="evenodd" clip-rule="evenodd" fill="none" stroke="#51C0DF" stroke-miterlimit="10" d="M47.807,0
                    c0,0-77.816,132.399-0.408,258.957"/>
            </g>
            <animateTransform attributeName="transform" attributeType="XML" type="rotate" from="90 250 250" to="-90 250 250" dur="3s" repeatCount="indefinite"/>
        </g>
    </svg>
</body>
</html>
````
![](http://gtms02.alicdn.com/tps/i2/TB1ktrxJXXXXXbIXFXXLalg4VXX-328-611.png)



