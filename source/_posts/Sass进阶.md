---
title: Sass进阶
date: 2023-11-06 23:55:14
categories: 
- web前端
tags:
- Sass
---
# sass进阶（包含css原子化内容）

Created: November 6, 2023 10:18 PM
Class: sass
Type: vue
Reviewed: No

## SASS

在前端开发中sass是必不可少的技能，在开发中对sass最明显的体验就是css嵌套以及变量功能，但是Sass（Syntactically Awesome Stylesheets）作为一种CSS的扩展语言，其内部还有很多增强和有用的功能，以简化和改进CSS代码的编写和维护。Sass具有以下主要特点：

1. **变量**sass允许你定义变量来存储颜色，字体，边距等css属性的值，使得在整个样式表中一致地使用这些变量变得容易，可以轻松复用。
2. 嵌套规则：可以嵌套CSS规则，以更清晰地表示HTML元素之间的层次关系。这有助于减少代码的嵌套深度，提高可读性（但是需要避免过深嵌套）
3. 混合器：混合器允许你定义可重用的样式片段，并在多个地方引用它们。这有助于减少代码的重复，使样式更易维护。
4. 继承：Sass支持样式继承，允许一个选择器继承另一个选择器的属性。这有助于创建DRY（Don't Repeat Yourself）的样式表。
5. 模块化：Sass允许将样式表拆分为多个文件，然后将它们导入到主样式表中，以帮助组织和管理大型项目的样式。
6. 运算：Sass支持在样式中执行算术运算，包括加法、减法、乘法和除法。这使得可以动态计算属性的值。

sass文件已”.scss”或者“.sass”拓展名，生产环境西药编译成css文件，我们在使用webapack进行开发的时候，webpack承担了自动化编译的工作。总的来说，Sass是一种强大的CSS预处理器，它使CSS代码更加模块化、易维护和可读，同时提供了许多便捷的功能来提高前端开发的效率。

ps：这里需要注意一下sass和scss的区别

1. **语法规则**:
    - **Sass**: Sass使用缩进风格的语法，不使用大括号 **`{}`** 和分号 **`;`**。选择器和属性之间使用缩进来表示层次关系。
    - **Scss**: Scss使用类似于标准CSS的语法，使用大括号 **`{}`** 包围规则块，以及分号 **`;`** 来分隔属性。这使得Scss更接近标准CSS。
2. **文件扩展名**:
    - **Sass**: Sass文件使用 **`.sass`** 扩展名。
    - **Scss**: Scss文件使用 **`.scss`** 扩展名。
3. **易读性**:
    - **Sass**: 由于缩进风格的语法，Sass文件通常更加紧凑，但在视觉上可能不如Scss易读。
    - **Scss**: Scss更接近标准CSS，因此它通常更容易阅读和理解，特别是对那些熟悉CSS的开发者来说。
4. **兼容性**:
    - **Sass**: 由于不使用大括号和分号，Sass的语法在某些情况下可能会导致一些语法错误，需要开发者小心处理。
    - **Scss**: Scss更接近标准CSS，因此在语法上更容易转换和适应。

## sass基础使用

### sass安装

```jsx
npm install sass
```

使用sass将sass类型文件转换为css

```jsx
.box {
    .box-content {
        color: red;
    }
}
// 正常来说浏览器是不能直接识别嵌套css的，chrome v112 已经支持嵌套了
// 所以我们需要将scss变异成css
```

**sass提供四种编译选项**

```jsx
nested：嵌套缩进的css代码，它是默认值。
expanded：没有缩进的、扩展的css代码。
compact：简洁格式的css代码。
compressed：压缩后的css代码。
```

生产环境一般使用compressed，编译命令

```jsx
npx sass --style compressed index.scss style.css
```

编译结果如下：

![Untitled](/images/sass进阶/Untitled.png)

### sass使用变量

```jsx
$prim-color: green;
.box {
  .box-content {
    color: $prim-color;
  }
}
```

![Untitled](/images/sass进阶/Untitled1.png)

同样我们可以将变量嵌套在字符串中，需要写在#{}之中

```jsx
$prim-color: green;
$side: left;
.box {
  .box-content {
    color: $prim-color;
  }
  .box-side {
    margin-#{$side}: 30px;
  }
}
```

### 使用sass计算功能

```jsx
$prim-color: green;
$marginLeft: 50px;
$side: left;
.box {
  .box-content {
    color: $prim-color;
  }
  .box-side {
    margin-#{$side}: 30px;
    margin-left: calc(100vh - $marginLeft);
  }
}
```

![Untitled](/images/sass/Untitled2.png)

### sass继承功能，代码复用

```jsx
$prim-color: green;
$marginLeft: 50px;
$side: left;
.box {
  .box-content {
    color: $prim-color;
    background-color: red;
  }
  .box-side {
    @extend .box-content;
    margin-#{$side}: 30px;
    margin-left: calc(100vh - $marginLeft);
  }
}
```

![Untitled](/images/sass进阶/Untitled3.png)

### Mixin（可以复用的代码块）

```jsx
$prim-color: green;
$marginLeft: 50px;
$side: left;
.box {
  .box-content {
    color: $prim-color;
    background-color: red;
  }
  .box-side {
    @extend .box-content;
    margin-#{$side}: 30px;
    margin-left: calc(100vh - $marginLeft);
  }
}
```

![Untitled](/images/sass进阶/Untitled4.png)

mixin支持传参

```jsx
$prim-color: green;
$marginLeft: 50px;
$side: left;
@mixin flex-row {
  display: flex;
  flex-direction: row;
}
@mixin right($right: 40px) {
  margin-right: $right;
}
.box {
  .box-content {
    @include right();
    color: $prim-color;
    background-color: red;
  }
  .box-side {
    @extend .box-content;
    @include flex-row;
    @include right(30px);
    margin-#{$side}: 30px;
    margin-left: calc(100vh - $marginLeft);
  }
}
```

![Untitled](/images/sass进阶/Untitled5.png)

### sass内置颜色函数

```jsx
lighten(#cc3, 10%) // #d6d65c
darken(#cc3, 10%) // #a3a329
grayscale(#cc3) // #808080
complement(#cc3) // #33c
```

**`darken($color, $amount)`  将color颜色加深amount介于0-100%之间**

**`lighten($color, $amount)`** 将颜色 `$color` 变亮`$mount` 是介于 `0%` 到 `100%` (含) 之间的值。按照这个值增加 `$color` HSL 的亮度。

**`mix($color1, $color2, $weight: 50%)`** 将颜色 `$color1` 与 `$color2` 混合在一起生成新的颜色，`$weight` 与透明度决定了每个颜色在结果里占的比重。`$weight` 默认值为 `50%`。取值范围是介于 `0%` 到 `100%` (含) 之间的值。如果指定的比例是 `25%` ,这意味着第一个颜色所占比例为 `25%`，第二个颜色所占比例为 `75%`。

### 插入文件

```jsx
@import "index.scss";
@import "index.css";
```

### 循环语句

for循环

```jsx
@for $i from 1 through 10 {
  .margin-left-#{$i} {
    margin-left: #{$i}px;
  }
}
```

![Untitled](/images/sass进阶/Untitled6.png)

while循环

```jsx
$index: 1;
@while $index < 10 {
  .margin-right-#{$index} {
    margin-right: #{$index}px;
  }
  $index: $index + 1;
}
```

![Untitled](/images/sass进阶/Untitled7.png)

each命令

```jsx
@each $member in a, b, c, d {
　　　　.#{$member} {
　　　　　　background-image: url("/image/#{$member}.jpg");
　　　　}
　　}
```

### 自定义函数

```jsx
@function doubleValue($value) {
  @return $value * 2;
}
.box-side {
  border: doubleValue(10px) solid black;
}
```

![Untitled](/images/sass进阶/Untitled8.png)

## 进阶内容

sass变量作用域名

![Untitled](/images/sass进阶/Untitled9.png)

在这个例子中可以看出，Sass 中的变量没有块级作用域特性，而是随执行随覆盖随调用。调用的前面没有变量声明，就报错，有很多变量声明，就调用在它上面离它最近的变量值，使用时最好是在顶部声明，之后全局调用就可以了。

### 引用父级元素   &

可以用 `&` 符号代替父级选择器，超级实用，例如：

```jsx
$width: 120px;
.main {
  .content {
    width: $width;
    $width: 130px;
  }
  $width: 140px;
  .sidebar {
    width: $width;
    &::after {
      content: "测试";
    }
  }
}
```

![Untitled](/images/sass进阶/Untitled10.png)

## sass中的四则运算

Sass 支持 加法（＋）、减法（－）、乘法（＊）、除法（／）以及取余（％）

### 数值相加

```jsx
$width1: 100px;
$width2: 200px;
.main {
  width: $width1 + $width2;
}
```

输出

```jsx
.main{width:300px}/*# sourceMappingURL=style.css.map */
```

这里的变量具有字符单位，但是sass在进行计算的时候会进行计算并对单位进行处理。

### 字符串相加

```jsx
.main::after {
  content: "hello " + world;
  font-family: sans- + "serif";
}
```

输出

```jsx

.main::after{content:"hello world";font-family:sans-serif}/*# sourceMappingURL=style.css.map */
```

加法连接字符串时，对于引号的合并也有一定规则，如果前面字符串带有引号，后面字符串会自动包含在引号中，如果前面没有，后面带有引号的字符串也会去掉引号。

可以使用#{}进行加法数值运算

```jsx
$num: 12;
.main::before {
  content: "这是啥" + #{$num + 1};
}
```

输出

```jsx
.main::before{content:"这是啥13"}/*# sourceMappingURL=style.css.map */
```

### @extend和**@mixin**

@extend可以直接继承另一个选择器中的属性，如果只在 CSS 中去对每个结构实现 clearfix 的效果，要么把这段代码不停的复制到对应选择器中，产生大量冗余，要么就把这个结构选择器翻到上面去找 .clearfix 类，加在后面，费时费力。

@mixin 定义的是一个片段，这个片段可以是类似变量的一段文字一条属性，也可以是一整个选择器和内容，也可以是一个选择器的一部分 CSS 代码。此外还可以传递参数，通过参数生成不同代码。它需要配合 @inclde 命令来引用这段代码，类似复制的效果。@mixin 定义的内容，不会编译输出。

可以实现类似函数效果

```jsx
@mixin border-radius($radius: 5px) {
  -webkit-border-radius: $radius;
  -moz-border-radius: $radius;
  border-radius: $radius;
}
.content {
  @include border-radius(10px);
}
```

输出

```jsx
.content{-webkit-border-radius:10px;-moz-border-radius:10px;border-radius:10px}/*# sourceMappingURL=style.css.map */
```