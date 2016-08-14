## Compass

Compass是Sass的工具库（toolkit）

Sass本身是一个编译器，Compass是在它的基础上，封装了一系列有用的模块和模板，补充Sass的功能。它们之间的关系，有点像JavaScript和JQuery，Ruby和Rails
，python和Djanjo

### reset模块

@import "compass/reset";重置浏览器的默认样式

### css3模块

圆角

@include border-radius(5px);
@include border-corner-radius(top, left, 5px);//这个是左上角圆角样式的设置

透明

@include opacity(0.5);//提供兼容ie的透明度样式

行内区块

@include inline-block;//生成兼容ie的样式，有*标记的都是兼容ie的。

### layout模块

该模块是提供布局功能的



