## sass学习

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

