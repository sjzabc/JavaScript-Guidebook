﻿# 基础语法

canvas顾名思义是定义在浏览器中的画布。它不仅是一个普通的元素，更是一个强大的编程工具。它的出现已然超过了web基于文档的设计初衷，将网页这一形态的应用推向了另一个高度。利用canvas，可以开发出复杂的动画、动态图表、游戏等。关于canvas，有这样一句话——canvas就像是一场文艺复兴，将编程工作者彻底释放出创造力。

## 基本语法

在HTML中添加Canvas非常简单，只需要在HTML的<body>部分，添加上 `<canvas>` 标签就可以了

```html
<canvas>
    <p>The canvas element is not supported!</p>
</canvas>
```

现在，页面是一个完完全全的空白页面。Canvas的本意是画布，画布在HTML5中是透明的，是不可见的。

canvas支持HTML属性高度 `height` 和宽度 `width`，可以在开始和结束标签之间加入HTML来提供后备内容。

 - `height` 高度
 - `width` 宽度

```html
<canvas width="600" height="300">
    <p>The canvas element is not supported!</p>
</canvas>
```

　[注意]重置canvas的宽或高可以达到清空画布的效果
 
  [注意]通过CSS样式设置的宽高，是canvas元素的实际占据宽高；通过属性值设置的宽度，是canvas内部编程的设置宽高；如果没有通过CSS样式设置宽度，则canvas元素实际占据宽高等于内容编程设置宽度
  
  如果按照如下进行设置，则canvas的最终宽高为400*100，相当于内部元素宽度缩小2.5倍，高度缩小2倍

```javascript
    canvas.width = 1000;
    canvas.height = 200;
    canvas.style.width = '400px';  
    canvas.style.height = '100px'; 
```

　　如果按照如下进行设置，则canvas的最终宽高为400*40，相当于内部元素宽度和高度等比例缩小2.5倍

```javascript
    canvas.width = 1000;
    canvas.height = 200;
    canvas.style.width = '400px';  
```

## 绘图上下文

要在canvas上绘图，需要以下三个步骤

　　1、布置画布：通过添加 `<canvas>` 标签，添加canvas元素

　　2、获取画布：通过 `<canvas>` 标签的id，获得canvas对象

　　3、取得绘图上下文：通过canvas对象的 `getContext("2d")` 方法，获得2D环境；如果要获取三维上下文，使用 `webgl`

　　上面的三个步骤对应如下代码

```html
<canvas id=“canvas”></canvas>
<script>
var canvas = document.getElementById("canvas");
var context = canvas.getContext("2d")
</script>
```

### canvas坐标

　　使用2D上下文提供的方法可以绘制简单的2D图形，比如矩形、弧线和路径。2D上下文坐标开始于canvas元素的左上角，原点坐标是(0,0)。所有坐标值都基于这个原点计算，x值越大表示越靠右，y值越大表示越靠下。默认情况下，width和height表示水平和垂直两个方向上可用的像素数目。
　　
![canvas坐标][1]


## 填充和描边

2D上下文的两种基本绘图操作是**填充**和**描边**

填充是指用指定的样式(颜色、渐变和图像)填充图形；描边是只在图形的边缘画线

　　大多数2D上下文操作都会细分为填充和描边两个操作，而操作的结果取决于两个属性:`fillStyle`和`strokeStyle`。这两个属性的值可以是字符串、渐变对象或模式对象，而且它们的默认值都是 `#000000` 。如果为它们指定表示颜色的字符串值，可以使用CSS中指定颜色值的任何格式，包括颜色明、十六进制码、rgb、rgba、hsl或hsla。举个例子：

```javascript
var ctx = drawing.getContext('2d');

ctx.strokeStyle = color;
ctx.strokeStyle = gradient;
ctx.strokeStyle = pattern;

ctx.fillStyle = color;
ctx.fillStyle = gradient;
ctx.fillStyle = pattern;
```

**选项**

 - `color`：DOMString字符串，被转换成CSS颜色值
 - `gradient`：CanvasGradient对象（线性渐变或者放射性渐变）
 - `pattern`：CanvasPattern对象（可重复图像）

## 绘制矩形

下面先从最简单的矩形绘制开始说起，矩形是唯一一种可以直接在2D上下文中绘制的形状，与矩形相关的方法包括 `fillRect()`、`strokeRect()`、`clearRect()`。

这三个方法都能接收4个参数：

- 矩形起始点的x坐标
- 矩形起始点的y坐标
- 矩形宽度
- 矩形高度

这些参数的单位都是像素


 - `fillRect(x, y, width, height)`：画布上绘制的矩形会**填充**通过fillStyle属性指定的颜色。
 - `strokeRect(x, y, width, height)`：画布上绘制的矩形会使用通过strokeStyle属性指定**描边**颜色。
 - `clearRect(x, y, width, height)`：用于**清除画布上的矩形区域**。本质上这个方法可以把绘制上下文中的某一矩形区域变透明。通过绘制形状然后再清除指定区域，就可以生成有意思的效果。


## 绘制路径

绘制路径包括**开始绘制**、**实际绘制**和**结束绘制**三个步骤

【开始绘制】

　　要绘制路径，首先必须调用 `beginPath()` 方法，表示要开始绘制新路径

　　[注意] `beginPath()` 之后的 `strokeStyle` 或 `fillStyle` 用于当前路径

【实际绘制】

　　实际绘制路径时可以使用以下方法：

　　1、`moveTo(x,y)`：将绘图游标移动到(x,y)，不画线。如果其他方法需要使用上一点的坐标，一定要先使用 `moveTo(x,y)` 确定坐标

```javascript
context.moveTo(100,100)
```

　　表示移动画笔至(100,100)这个点（单位是px）

　　2、`lineTo(x,y)`：从上一点开始绘制一条直线，到(x,y)为止

```javascript
context.lineTo(600,600)
```

　　表示从上一笔的停止点绘制到(600,600)

　　3、`arcTo(x1,y1,x2,y2,radius)`：从上一点开始绘制一条弧线到(x2,y2)为止，并以给定半径radius穿过(x1,y1)

```javascript
context.arcTo(30,80,100,100,60);
```

　　表示从上一点开始绘制一条弧线到(100,100)为止，该弧线穿过(30,80)，且半径为60

　　4、`arc(x,y,radius,startAngle,endAngle,counterclockwise)`：以(x,y)为圆心绘制一条弧线，弧线半径为radius，起始和结束角度(用弧度表示)分别为 `startAngle` 和 `endAngle` 。最后一个参数表示 `startAngle` 和 `endAngle` 是否按逆时针方向计算。默认值为 `false` 表示按顺时针方向计算

```javascript
context.arc(50,50,40,0,2*Math.PI,false);
```

　　表示以(50,50)为圆心绘制一条弧线，半径为40，起始和结束角度分别为0和2PI，按顺时针方向计算

　　5、`bezierCurveTo(c1x,c1y,c2x,c2y,x,y)`：从上一点开始绘制一条曲线，到(x,y)为止，并且以(c1x,c1y)和(c2x,c2y)为控制点

```javascript
context.bezierCurveTo(0,50,100,50,100,0);
```

　　表示从上一点开始绘制一条曲线，到(100,0)为止，并且以(0,50)和(100,50)为控制点

　　6、`quadraticCurveTo(cx,cy,x,y)`：从上一点开始绘制一条二次曲线，到(x,y)为止，并且以(cx,cy)为控制点

```javascript
context.quadraticCurveTo(50,50,0,100);
```

　　表示从上一点开始绘制一条二次曲线，到(0,100)为止，并且以(50,50)为控制点

　　7、`rect(x, y, width, height)`：从点(x,y)开始绘制一个矩形，宽度和高度分别由width和height指定。这个方法绘制的是矩形路径，而不是strokeRect()和fillRect()所绘制的独立的形状

```javascript
context.rect(20,20,50,50);
```

　　表示从(20,20)开始绘制一个矩形，宽高分别是50和50

【结束绘制】

　　创建路径后有以下4种选择

　　1、用 `fillStyle` 填充，调用 `fill()` 方法

　　2、用 `strokeStyle` 描边，调用 `stroke()` 方法

　　[注意]如果`fill()`和`stroke()`同时使用，应该先使用`fill()`，后使用`stroke()`。否则，`fill()`会覆盖`stroke()`的部分线条宽度

　　3、在路径上创建一个剪切区域，调用clip()方法

　　[注意]canvas中的 `clip()` 方法用于从原始画布中剪切任意形状和尺寸。一旦剪切了某个区域，则所有之后的绘图都会被限制在被剪切区域内(不能访问画布上的其他区域)。也可以在使用 `clip()` 方法前通过使用 `save()` 方法对当前画布区域进行保存，并在以后任意时间通过 `restore()` 方法对其进行恢复。可以使用 `clip()` 实现类似于探照灯效果

　　4、绘制一条连接到路径起点的线条，调用 `closePath()` 方法

　　在2D绘图上下文中，路径是一种主要的绘图方式，因为路径能为要绘制的图形提供更多控制。由于路径的使用很频繁，所以有一个 `isPointInPath()` 方法，接收x和y坐标作为参数，用于在路径被关闭之前确定画布上的某一点是否位于路径上

```javascript
if(context.isPointInPath(100,100)){
    console.log('this point is in the path');
}
```

## 绘制文本

绘制文本主要有两个方法：`fillText()` 和 `strokeText()`

```javascript

var ctx = drawing.getContext('2d');

ctx.fillText(text, x, y, [, maxWidth]);
ctx.strokeText(text, x, y)
```

 - `text`：使用当前的 `font`, `textAlign`, `textBaseline` 和 `direction` 值对文本进行渲染。
 - `x`：文本起始点的x轴坐标
 - `y`：文本起始点的y轴坐标
 - `maxWidth`：（可选）绘制的最大宽度。如果指定了值，并且经过计算字符串的值比最大宽度还要宽，字体为了适应会水平缩放（如果通过水平缩放当前字体，可以进行有效的或者合理可读的处理）或者使用小号的字体。

这两个方法都可以接收4个参数：要绘制的文本字符串、x坐标、y坐标和可选的最大像素宽度，而且，这两个方法都以下列3个属性为基础。

- font: 表示文本样式、大小及字体，用CSS中指定字体的格式来指定，例如"10px Arial"。
- textAlign：表示文本对齐方式。可能的值有 `start`、`end`、`left`、`right`和`center`。建议使用 `start` 和 `end`，不要使用 `left` 和 `right`，因为前两者的意思更稳妥，能同时适合从左到右和从右到左显示（阅读）的语言。
- textBaseline：表示文本的基线。可能的值有 `top`、`hanging`、`middle`、`alphabetic`、`ideographic`和`bottom`。

这几个属性都有默认值，因此没有必要每次使用它们都重新设置一遍值。`fillText()` 方法使用 `fillStyle` 属性绘制文本，而 `strokeText()` 方法使用 `strokeStyle` 属性为文本描边。相对来说，还是使用 `fillText()` 的时候更多，因为该方法模仿了在网页中正常显示文本。


## 变换

### 旋转变换rotate

`CanvasRenderingContext2D.rotate()` 是 Canvas 2D API 在变换矩阵中增加旋转的方法。角度变量表示一个顺时针旋转角度并且用弧度表示。

> void ctx.rotate(angle);

**参数**

 - `angle`：顺时针旋转的弧度。如果你想通过角度值计算，可以使用公式： `degree * Math.PI / 180` 。

旋转中心点一直是 canvas 的起始点。 如果想改变中心点，我们可以通过 `translate()` 方法移动 canvas 。

![rotate][2]

```html
<canvas id="canvas"></canvas>
```
```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.rotate(45 * Math.PI / 180);
ctx.fillRect(70,0,100,30);

// reset current transformation matrix to the identity matrix
ctx.setTransform(1, 0, 0, 1, 0, 0);
```
 
### 缩放变换scale

`CanvasRenderingContext2D.scale()` 是 Canvas 2D API 根据 x 水平方向和 y 垂直方向，为canvas 单位添加缩放变换的方法。

默认的， 在 canvas 中一个单位实际上就是一个像素。例如，如果我们将0.5作为缩放因子，最终的单位会变成0.5像素，并且形状的尺寸会变成原来的一半。相似的方式，我们将2.0作为缩放因子，将会增大单位尺寸变成两个像素。形状的尺寸将会变成原来的两倍。

> void ctx.scale(x, y);

**参数**

- `x`：水平方向的缩放因子。
- `y`：垂直方向的缩放因子。

```html
<canvas id="canvas"></canvas>
```
```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.scale(10, 3);
ctx.fillRect(10,10,10,10);

// reset current transformation matrix to the identity matrix
ctx.setTransform(1, 0, 0, 1, 0, 0);
```

### 平移变换translate

`CanvasRenderingContext2D.translate()` 是 Canvas 2D API 通过在网格中移动 canvas 和 canvas 原点 x 水平方向、原点 y 垂直方向，添加平移变换的方法。

> void ctx.translate(x, y);

**参数**

 - `x`：水平方向的移动距离。
 - `y`：垂直方向的移动距离。

![translate][3]

```html
<canvas id="canvas"></canvas>
```

```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.translate(50, 50);
ctx.fillRect(0,0,100,100);

// reset current transformation matrix to the identity matrix
ctx.setTransform(1, 0, 0, 1, 0, 0);
```

### 自由变换transform

`CanvasRenderingContext2D.transform()` 是 Canvas 2D API 使用矩阵多次叠加当前变换的方法，矩阵由方法的参数进行描述。你可以缩放、旋转、移动和倾斜上下文。

参见 `setTransform()` 方法，这个方法使用单位矩阵重新设置当前的变换并且会调用 `transform()`。

> void ctx.transform(a, b, c, d, e, f);
 

**参数**

 - `a (m11)`：水平缩放。

 - `b (m12)`：水平倾斜。

 - `c (m21)`：垂直倾斜。

 - `d (m22)`：垂直缩放。

 - `e (dx)`：水平移动。

 - `f (dy)`：垂直移动。

```html
<canvas id="canvas"></canvas>
```

```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.transform(1,1,0,1,0,0);
ctx.fillRect(0,0,100,100);
```
 
### setTransform

`CanvasRenderingContext2D.setTransform()` 是 Canvas 2D API 使用单位矩阵重新设置（覆盖）当前的变换并调用变换的方法，此变换由方法的变量进行描述。

参见 `transform()` 方法，这个方法不会覆盖当前的变换矩阵，会多次叠加变换。

> void ctx.setTransform(a, b, c, d, e, f);

**参数**

 - `a (m11)`：水平缩放。
 - `b (m12)`：水平倾斜。
 - `c (m21)`：垂直倾斜。
 - `d (m22)`：垂直缩放。
 - `e (dx)`：水平移动。
 - `f (dy)`：垂直移动。



```html
<canvas id="canvas"></canvas>
```
```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

ctx.setTransform(1,1,0,1,0,0);
ctx.fillRect(0,0,100,100);
```

## 绘制图像

### drawImage

`CanvasRenderingContext2D.drawImage()` 是 Canvas 2D API 中的方法，它提供了多种方式来在Canvas上绘制图像。

> void ctx.drawImage(image, dx, dy);
void ctx.drawImage(image, dx, dy, dWidth, dHeight);
void ctx.drawImage(image, sx, sy, sWidth, sHeight, dx, dy, dWidth, dHeight);
drawImage

**参数**

 - `image`：（要绘制的图像）绘制到上下文的元素。允许任何的 canvas 图像源(CanvasImageSource)，例如：`HTMLImageElement`，`HTMLVideoElement`，或者 `HTMLCanvasElement`。
 - `dx`：（源图像的x坐标）目标画布的左上角在目标canvas上 X 轴的位置。
 - `dy`：（源图像的y坐标）目标画布的左上角在目标canvas上 Y 轴的位置。
 - `dWidth`：（源图像的宽度）在目标画布上绘制图像的宽度。 允许对绘制的图像进行缩放。 如果不说明， 在绘制时图片宽度不会缩放。
 - `dHeight`：（源图像的高度）在目标画布上绘制图像的高度。 允许对绘制的图像进行缩放。 如果不说明， 在绘制时图片高度不会缩放。
 - `sx`：（目标图像的x坐标）需要绘制到目标上下文中的，源图像的矩形选择框的左上角 X 坐标。
 - `sy`：（目标图像的y坐标）需要绘制到目标上下文中的，源图像的矩形选择框的左上角 Y 坐标。
 - `sWidth`：（目标图像的宽度）需要绘制到目标上下文中的，源图像的矩形选择框的宽度。如果不说明，整个矩形从坐标的sx和sy开始，到图像的右下角结束。
 - `sHeight`：（目标图像的高度）需要绘制到目标上下文中的，源图像的矩形选择框的高度。

![drawImage][4]

```html
<canvas id="canvas"></canvas>
<div style="display:none;">
  <img id="source" src="https://mdn.mozillademos.org/files/5397/rhino.jpg"
       width="300" height="227">
</div>
```

```javascript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");
var image = document.getElementById('source');

ctx.drawImage(image, 33, 71, 104, 124, 21, 20, 87, 104);
```

结合使用 `drawImage()` 和其他方法，可以对图像进行进行各种基本操作。而操作的结果可以通过 `toDataURL()` 方法获得。不过，有一个例外，即图像不能来自其他域。如果图像来自其他域，调用 `toDataURL()` 会抛出一个错误。
 
## 阴影

2D上下文会根据以下几个属性的值，自动为形状或路径绘制出阴影。

 - `shadowColor`：用CSS颜色格式表示的阴影颜色，默认为黑色
 - `shadowOffsetX`：形状或路径x轴方向的阴影偏移量，默认为0
 - `shadowOffsetY`：形状或路径y轴方向的阴影偏移量，默认为0
 - `shadowBlur`：模糊的像素数，默认0，即不模糊
 
这些属性度可以通过 `context`对象来修改。只要在绘制前为它们设置适当的值，就能自动产生阴影。

```javascript
const context = drawing.getContext('2d');

// 设置阴影
context.shadowOffsetX = 5;
context.shadowOffsetY = 5;
context.shadowBlur = 4;
context.shadowColor = 'rgba(0, 0, 0, 0.5)';

// 绘制红色矩形
context.fillStyle = '#ff0000';
context.fillRect(10, 10, 50, 50);

// 绘制蓝色矩形
context.fillStyle = 'rgba(0, 0, 255, 1)';
context.fillRect(30, 30, 50, 50);
```

## 渐变

渐变
　　填充和描边除了可以取颜色值之外，还可以取渐变值，渐变由 `canvasGradient` 实例表示。

### 创建渐变

　　渐变分为线性渐变和径向渐变

　　`CanvasRenderingContext2D.createLinearGradient()` 方法创建一个沿参数坐标指定的直线的渐变。该方法返回一个线性 `CanvasGradient` 对象。

> CanvasGradient ctx.createLinearGradient(x0, y0, x1, y1);

**参数**

 - `x0`
    起点的 x 轴坐标。
 - `y0`
起点的 y 轴坐标。
 - `x1`
终点的 x 轴坐标。
 - `y1`
终点的 y 轴坐标。

**返回值**

  - `CanvasGradient`

一个根据指定线路初始化的线性 `CanvasGradient` 对象。

　　[注意]如果想从某个形状的中心点开始创建一个向外扩散的径向渐变效果，要将两个圆定义为同心圆
　　
　　使用 `createLinearGradient` 方法
这是一段简单的代码片段，使用 `createLinearGradient` 方法创建一个指定了开始和结束点的 `CanvasGradient` 对象。创建成功后， 你就可以使用 `CanvasGradient.addColorStop()` 方法，根据指定的偏移和颜色定义一个新的终止。 如例子所示，渐变允许赋值给当前的 `fillStyle` ，使用 `fillRect()` 方法时，在 canvas 上绘制出效果。

```html
<canvas id="canvas"></canvas>
```

```javaScript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var gradient = ctx.createLinearGradient(0, 0, 200, 0);
gradient.addColorStop(0, "green");
gradient.addColorStop(1, "white");
ctx.fillStyle = gradient;
ctx.fillRect(10, 10, 200, 100);
```

### 指定色标

　　接下来使用 `addColorStop()` 方法来指定色标。这个方法接收两个参数：色标位置和CSS颜色值。色标位置是一个0(开始的颜色)到1(结束的颜色)之间的数字

　　最后将渐变对象实例赋值给 `fillStyle` 或 `strokeStyle`，进而可以绘制图形。

　　下面来创建一个垂直方向的从品红到浅蓝色的线性渐变

```html
<canvas id="drawing" style="border:1px/ solid black">
    <p>The canvas element is not supported!</p>
</canvas>

<script>
var drawing = document.getElementById('drawing');
if(drawing.getContext){
    var context = drawing.getContext('2d');
    var linearGradient = context.createLinearGradient(0,0,0,100);    
    linearGradient.addColorStop(0,'pink');
    linearGradient.addColorStop(1,'lightblue');
    context.strokeStyle = linearGradient;
    context.fillStyle = linearGradient;
    context.fillRect(10,10,100,100);
    context.strokeRect(120,10,100,100);
    context.font="20px/50px 宋体";
    context.textAlign = 'end';
    context.textBaseline = 'top';
    context.strokeText("小火柴",290,10);    
} 
</script>
```

　　下面来创建一个从品红到浅蓝色的径性渐变

```html
<canvas id="drawing" style="border:1px solid black">
    <p>The canvas element is not supported!</p>
</canvas>
<script>
var drawing = document.getElementById('drawing');
if(drawing.getContext){
    var context = drawing.getContext('2d');
    var radialGradient = context.createRadialGradient(50, 50, 0, 50, 50, 50);
    radialGradient.addColorStop(0,'pink');
    radialGradient.addColorStop(1,'lightblue');
    context.fillStyle = radialGradient;
    context.fillRect(0, 0, 100, 100);
}
</script>
```

## 模式

`CanvasRenderingContext2D.createPattern()` 是 Canvas 2D API 使用指定的图像 (CanvasImageSource)创建模式的方法。 它通过 repetition 参数在指定的方向上重复元图像。此方法返回一个CanvasPattern对象。

> CanvasPattern ctx.createPattern(image, repetition);

**参数**

 - `image`
作为重复图像源的 CanvasImageSource 对象。可以是下列之一：
    
     - HTMLImageElement (<img>),
     - HTMLVideoElement (<video>),
     - HTMLCanvasElement (<canvas>),
     - CanvasRenderingContext2D,
     - ImageBitmap,
     - ImageData,
     - Blob.

 - `repetition`
    - DOMString，指定如何重复图像。允许的值有：
        - "repeat" (both directions),
        - "repeat-x" (horizontal only),
        - "repeat-y" (vertical only), 
        - "no-repeat" (neither).

如果为空字符串 ('') 或 `null` (但不是 `undefined`)，repetition将被当作"repeat"。

**返回值**

  - `CanvasPattern`
    描述模式的不透明对象

这是一段简单的代码片段，使用 createPattern 方法创建一个指定图像和重复的 CanvasPattern 对象。创建完成后，可以使用 `CanvasPattern.setTransform()` 方法对图案进行变形。如示例所示，你可以把此模式赋值给当前的 `fillStyle`，当你使用 `fillRect()` 方法时，会在 canvas 上绘制出效果。

```html
<canvas id="canvas"></canvas>
```
```javaScript
var canvas = document.getElementById("canvas");
var ctx = canvas.getContext("2d");

var img = new Image();
img.src = 'https://mdn.mozillademos.org/files/222/Canvas_createpattern.png';
img.onload = function() {
  var pattern = ctx.createPattern(img, 'repeat');
  ctx.fillStyle = pattern;
  ctx.fillRect(0,0,400,400);
};
```

## 使用图像数据



## 合成


  [1]: https://images2015.cnblogs.com/blog/740839/201512/740839-20151219104712006-1891188943.jpg
  [2]: https://mdn.mozillademos.org/files/233/Canvas_grid_rotate.png
  [3]: https://mdn.mozillademos.org/files/234/Canvas_grid_translate.png
  [4]: https://mdn.mozillademos.org/files/225/Canvas_drawimage.jpg