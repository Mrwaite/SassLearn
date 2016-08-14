## sass学习
<div class="note info"><h2 id=""></h2></div>
### 使用变量 

sass使用$符号来标识变量
$声明的变量是有块级作用域的
sass的变量名可以与css中的属性名和选择器名称相同
sass并不想强迫任何人一定使用中划线或下划线，所以这两种用法相互兼容，就是
$link-color: blue;
a {
  color: $link_color;
}

//编译后

a {
  color: blue;
}
中划线命名的内容和下划线命名的内容是互通的，除了变量，也包括对混合器和Sass函数的命名，但是定义的css的ID，类，属性名不是互通的

### 嵌套css规则

sass会像俄罗斯套娃一样一层一层的，把父选择器放在子选择器前面，形成一条一条规则（但是这样的方案，根据css样式从右到左解析来说，效率不太好）
&父选择器标识符，

当用户在使用IE浏览器时，你会通过JavaScript在<body>标签上添加一个ie的类名
#content aside {
  color: red;
  body.ie & { color: green }
}

群组选择器可以让这样的
.container h1, .container h2, .container h3 { margin-bottom: .8em }
书写成这样
.container {
  h1, h2, h3 {margin-bottom: .8em}
}

article {
  nav + & {margin-left : 50px;}
}
解析之后是
nav + article { margin-top: 0 }
并不是 articles nav + articles {margin-left : 50px;}

属性的嵌套：
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
===》
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}

### 导入SASS文件

css原来由@import导入其他文件的功能，只有执行到@import的时候才会取导入文件
SASS也有@import的功能，但是SASS是在SASS文件编译成css文件的时候就已经导入了，而且导入的文件是全局变量都可以使用

3.1使用SASS部分文件
sass局部文件的文件名以下划线开头
在@import局部文件的时候可以不用写文件开头的下划线
比如你想导入_night.sass,在文件中只需要写@import "night.sass"

3.2 默认变量值
$link-color : blue !default;
!default含义是：如果这个变量被声明赋值了，那就用它声明的值。否则就用这个默认值
 
3.3 嵌套导入
可以这样是用sass的导入
nav {
    @impo
    >rt "nest.scss";
}
这样nest.scss文件的内容会导入nav选择器里面，导入的nest.scss文件定义的所有变量和混合器都只会在nav作用域中生效。
3.4原生css导入

### 注释
//单行注释编译完之后是看不到的
/*  */ 多行注释编译之后是看得到的

### 混合器

使用@minin来定义混合器的名称，调用的时候用@include，就会把相应混合器中的代码填入其中

5.1何时使用混合器
判断一组代码是否可以组成一个混合器，一条经验法则就是你能否为这个混合器相处一个好名字，能，就能说明你提取出来的这样的一组css样式能行驶某方面的职责
混合器和css样式的类有一定的相似性，但是html当中的类名应当描述这个模块的功能方面的东西，而混合器描述的是这块css代码最终所呈现的效果的描述。

5.2 混合器中的css规则
混合器不仅仅只能包括css样式，还可以包括css选择器。

5.3给混合器传参
就像函数给mixin设定形参，但是参数的命名要以$开头
除了按照形参的顺序传递参数，开可以以这样的形式传递参数
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
    @include link-colors(
      $normal: blue,
      $visited: green,
      $hover: red
  );
}
这样就没有参数的顺序问题，感觉这样的设定还是挺吊的

5.4默认参数
可以给函数设定默认值
@mixin link-colors(
    $normal,
    $hover: $normal,
    $visited: $normal
  )
{
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}

混合器就是函数形式的样式展示层的重用

### 使用选择器集成来精简css

可以使用@extend集成所有的样式，包括和其相关的所有的组合选择器样式
example ： 
.error {
  border : 1px red;
  background : #fdd;
}

.error a {
  list-style-type: none;
}

h1.error {
  color : red;
}

.seriousError {
  @extend .error;
  border-width : 3px;
}

==>

.error, .seriousError {
  border: 1px red;
  background: #fdd; }

.error a, .seriousError a {
  list-style-type: none; }

h1.error, h1.seriousError {
  color: red; }

.seriousError {
  border-width: 3px; }

看到上面就之道extend到底是怎么运行的，这时候就会考虑如果继承下来的样式和原来的样式有冲突，他们的前后顺序是怎么样的
就算是被继承的样式写在后面也是可以的继承的

6.1何时使用继承
你发现你的某个类（比如说.seriousError）另一个类（比如说.error）的细化。你会怎么做？

你可以为这两个类分别写相同的样式，但是如果有大量的重复怎么办？使用sass时，我们提倡的就是不要做重复的工作。
你可以使用一个选择器组（比如说.error.seriousError）给这两个选择器写相同的样式。如果.error的所有样式都在同一个地方，这种做法很好，但是如果是分散在样式表的不同地方呢？再这样做就困难多了。
你可以使用一个混合器为这两个类提供相同的样式，但当.error的样式修饰遍布样式表中各处时，这种做法面临着跟使用选择器组一样的问题。这两个类也不是恰好有相同的 样式。你应该更清晰地表达这种关系。
综上所述你应该使用@extend。让.seriousError从.error继承样式，使两者之间的关系非常清晰。更重要的是无论你在样式表的哪里使用.error``.seriousError都会继承其中的样式。

6.3继承的工作细节

关于继承，你需要知道两点：
1.继承重复的是选择器，但是混合器重复的是属性，继承的代码量更少一点
2.就是我上面说的继承之后可能要是会有冲突，看权重就好了

6.4继承的最佳实践
继承中不要使用后代选择器
.foo .bar { @extend .baz; }
.bip .baz { a: b; }
===>
.bip .baz, .bip .foo .bar, .foo .bip .bar {
  a: b; }





