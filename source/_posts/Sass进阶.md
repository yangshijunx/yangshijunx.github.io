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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled.png)

### sass使用变量

```jsx
$prim-color: green;
.box {
  .box-content {
    color: $prim-color;
  }
}
```

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%201.png)

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

[https://www.notion.so](https://www.notion.so)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%202.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%203.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%204.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%205.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%206.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%207.png)

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

![Untitled](sass%E8%BF%9B%E9%98%B6%EF%BC%88%E5%8C%85%E5%90%ABcss%E5%8E%9F%E5%AD%90%E5%8C%96%E5%86%85%E5%AE%B9%EF%BC%89%204d10b02eaa754f248e76fc1ef59eaf9a/Untitled%208.png)