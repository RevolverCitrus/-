# ==CSS==

## css简介

-层叠样式表

-网页其实是一个多层的结构，通过css可以分别为网页的每一个层来设置样式-而最终我们能看到只是网页的最上边一层

 

### 使用css来修改元素的样式

#### 方法1（内联样式，行内样式）

-在标签内部通过style属性来设置元素的样式

```
<p style="color: red;font-size: 60px;"> 君不见黄河之水天上来</p>
```

==问题==：

内联样式只能对一个标签生效，如果希望影响到多个元素就需要在每个元素添加

并且当样式发生变化时我们要一个一个进行修改，所以非常不方便

所以开发时绝对不要使用内联样式

 

#### 方法2（内部样式）

将样式编写到head中的style标签里

然后通过css的选择器来选中元素并为其设置各种样式

可以同时为多个标签设置样式，并且修改时只需修改一处即可全部应用

内部样式更加方便对样式进行复用

==问题：==

内部样式只能对一个网页生效，不能跨页面复用

```
<style>
        p{
            color: green;
            font-size: 50px;
        }
</style>

```

 

 

####方法3（外部样式表）

开发时主要用这种方式

可以将css样式编写到一个外部的css文件中

然后通过link标签引入外部的css文件（放到head中），意味着只要想使用这些样式的网页都可以对其进行引用，使样式可以在不同的页面之间进行复用
```
<link rel="stylesheet" href="style.css">
```
将样式编写到外部的css文件中，可以使用到浏览器的缓存机制，从而加快网页的加载速度，提高用户体验

------



##css的语法：

### 选择器{声明块}

选择器：通过声明块可以选中页面中的指定元素，比如p的作用就是选中页面中的所以的p元素

声明块：通过声明块来指定要为元素设置的样式

声明块由一个一个的声明组成

声明是一个名值对结构

一个样式名对应一个样式值，名和值之间以：连接，以；结尾

 

### 选择器：

==元素选择器：==

作用：根据标签名来选中指定的元素

语法：标签名{} 如：p{} h1{}

==Id选择器：==

作用：根据元素的id属性值选中一个元素

语法：#id属性值{} 如：#red{}

==类选择器：==

作用：根据元素的class属性值选中一组元素

语法：.class属性值

==适配选择器：==

作用：选中页面中的所有元素

语法：*

```
<style>
        p{
            color: aqua;
            font-size: 50px;
        }
        #abc{
            color: chartreuse;
        }
        .aaa{
            color: darkblue;
        }
        *{
            color: deeppink;
        }
</style>
```

#### 复合选择器：

==交集选择器：（同时符合的才变）==

作用：选中同时复合多个条件的元素

语法：选择器1选择器2....选择器n{}

==注意点：==

交集选择器中如果有元素选择器，一定要把元素选择器写在选择器开头



==并集选择器：（符合的同时变）==

作用：同时选择多个选择器对应的元素

语法：选择器1，选择器2，....，选择器n{}

```
<style>
    div.red{
        font-size: 30px;
    }
    #ac,.aaa{
        color: red;
    }
</style>
```

 

#### 关系选择器：

==子元素选择器;==

作用：选中指定父元素的指定子元素

语法：父元素>子元素

==后代选择器：==

作用：选中指定元素内的指定后代元素

语法：祖先 后代

==选择下一个兄弟==

语法：前一个+下一个

选择下边的所有兄弟

语法：兄 ~ 弟

    <style>
     	p>span{
           color: red;
        }
        div .aaa{
           font-size: 50px;
        }
        p+span{
           color: blue;
        }
        p~div{
           font-size: 100px;
        }
    </style>
    	<div>
    	     我是一个div
            <p>我是div里面的p
                <span>我是p中的spen</span>
            </p>
            <span class="aaa">我是div中的spen</span>
            <div>今天天气不错！</div>
        </div>

 


#### 属性选择器：

[属性名]：选择含有指定属性的元素

[属性名=属性值]：选择含有指定属性和属性值的元素

[属性名^=属性值]：选择属性值以指定值开头的元素

[属性名＄=属性值]：选择属性值以指定值结尾的元素

[属性名*=属性值]：选择属性值中含有某值的元素

 

### 选择器的权重(优先级)

==样式冲突==：当我们通过不同的选择器，选中相同的元素，并且为相同的样式设置不同的值时，此时就会发生样式冲突

发生样式冲突时，应用哪个样式由选择器的权重（优先级）决定

权重：内联样式 (1,0,0)

Id选择器 (0,1,0)

类和伪类选择器(0,0,1)

元素选择器 1

通配选择器 0

继承的样式 没有优先级

比较优先级时，需要将所有选择器的优先级进行相加计算，最后优先级越高，则越优先显示（但分组选择器是单独计算的）

但优先级的累加不会超过其最大的数量级，如再多的类选择器相加，最后也不会超过id选择器的优先级

如果优先级计算后相同，此时优先使用靠下的样式

可以在某个样式的后面加上 ！important  ，则此时该样式的优先级为最高，超过内联样式。但开发中要慎用！

```
    <style>
        #aaa{
            background-color: red;
        }
        .bbb{
            background-color: blue !important;
        }
        p{
            background-color: black;
        }
    </style>
```



### ==伪类==

（不存在的类，特殊类）

-伪类用来描述一个元素的特殊状态，比如第一个子元素

伪类一般情况下都是所有：开头

：first-child 第一个子元素

：last-child 最后一个子元素

：nth-child()选中第n个子元素

特殊值：

n 第n个 n的范围0到正无穷

2n 或 even 表示选中偶数位的元素

2n+1 或 odd 表示选中奇数位的元素

以上这些伪类都是根据所有的子元素进行排序

 

==：first-of-type== 

==：last-of-type== 

==：nth-of-type()==

​	以上几个伪类的功能和上述的基本相同，不通点是他们是在同类型元素中进行排序

 

==：not()否定伪类==：

​	将符合条件的元素从选择器中去除

```
ul>li:last-child{
     color: red;
    }
ul>li:not(:last-child){
      color: red;
   }
```

### ==伪元素==

表示页面中一些特殊的==并不真实的存在的元素==（特殊的位置）

伪元素使用：：开头

：：first-letter表示第一个字母

：：first-line表示第一行

：：selection表示选中的内容

==：：before==元素的开始
		==：：after==元素的最后

before和after必须结合content属性来使用

```
<style>
        p::first-letter{
            font-size: 50px;
        }
        p::first-line{
            color: blue;
        }
        p::selection{
            color: green;
        }
        p::before{
            content: "hhhh";
            color: cyan;
        }
        p::after{
            content: "eeee";
            color: aqua;
        }
</style>
```



### 超链接的伪装

==：link==用来表示没访问过的链接（正常的链接）

==：visited==用来表示访问过的链接

由于隐私的原因，所有visited这个伪类只能修改链接的颜色，不能改大小

==：hover==用来表示鼠标移入的状态

==：active==用来表示鼠标点击

```
 <style>
        a:link{
            color: red;
        }
        a:visited{
            color: green;
        }
        a:hover{
            color: blue;
        }
        a:active{
            font-size: 80px;
        }
</style>
```



### 样式的继承

我们为一个元素设置的样式同时也会应用到它的后代元素上

继承是发生在祖先后后代之间的

利用继承我们可以将一些通用的样式统一设置到共同的祖先元素上，这样只需设置一次即可让所有的元素都具有该样式

==注意==：并不是所有的样式都会被继承

比如背景相关的，布局相关的等这些样式都不会被继承

 

------



## 文档流（normal flow）

网页是一个多层结构，通过css可以为每一层设置样式，用户只能看到最顶一层

这些层中最底层称为文档流，文档流是网页的基础，我们创建的元素默认都是在文档流中进行排序



==元素主要有两个状态==：

在文档流中，不在文档流中==（脱离文档流）==



元素在文档流中的有什么特点：

==块元素==：在页面中独站一行，默认宽度是父元素的全部，默认高度是被内容撑开

==div、p、h1~h6、ul、li==、dl、ol、table、hr、table，

HTML5新增的header、section、aside、footer

==行内元素==：只是站自身的大小，默认高度和宽度都是被内容撑开

==span、img、a、input==、em（强调）、big、i（斜体）、textarea、strong

button（默认display：inline-block）

 

------



## ==盒子模型==（box model）

盒模型，框模型

css将页面中的所有元素都设置为一个矩形的盒子

将元素设置为矩形的盒子后，对页面的布局就变成将不同的盒子摆放到不同位置

每一个盒子都由以下几部分组成：

==内容区（content）==

==内边距（padding）==

==边框（border）==

==外边距（margin）==

 

### 内容区（content）

> ​	元素中所有的子元素和文本内容都在内容区中排列

内容区的大小由width和height两个属性来设置

width设置内容区的大小

height设置内容区的高度

 

### 边框（border）

> ​	边框属于盒子边缘，边框里边属于盒子内部，边框外边属于盒子外部

边框的大小会影响整个盒子的大小

要设置边框，需要至少设置三个样式：

边框的宽度 border-width

边框的颜色 border-color

边框的样式 border-style

```
    <style>
        .aaa{
            width: 200px;
            height: 200px;
            background-color: greenyellow;

            border-width: 10px;
            border-color: red;
            border-style: solid;
        }
    </style>
```

 

border-width:有默认值，一般是3px

它可以指定四个方向的边框宽度

情况：4个值：上 右 下 左

3个值：上 左右 下

2个值：上下 左右

1个值：上下左右

除了border-width还有一组border-xxx-width

xxx可以是top right bottom left

可以用来单独指定某个边的宽度

border-width: 10px 20px 30px 40px;

border-top-width: 50px;

 

border-color：用来指定边框的颜色，同样可以分别指定四个边框的颜色

规则和border-width一样

它也可以不写，如果不写则会自动使用color的颜色值

border-color: red blue green yellow;

 

border-style：指定边框的样式，也有默认值即none （没有）

solid 表示实线

dotted点状虚线

dashed虚线

double双线

同样可以分别指定四个边框的样式

也有border-xxx-style

top right bottom left

border-top-style: dashed;

border-style: solid dashed dotted double;

 

==border简写属性==：通过该属性可以同时设置边框所有的相关样式，并没有顺序要求

border: 10px green solid ;

同样也有border-xxx  

top right bottom left

 

### 内边距（padding）

> ​	内容区和边框之间的距离

一共有四个方向：

padding-top

padding-right

padding-bottom

padding-left

内边距也会影响盒子的大小，而且背景颜色会延伸到内边距上

盒子的可见框的大小（即盒子的大小），由内容区，内边框，边框共同组成

padding内边距的简写属性也和border-width一样

 

 

### 外边距（margin）

外边距不会影响盒子可见框的大小

但会影响盒子的位置

一个有四个位置：

margin-top：

上外边距，设置一个正值，元素会向下移动

margin-right：

默认情况下设置margin-right不会产生任何效果

margin-bottom：

下外边距，设置一个正值，其下边的其它元素会向下移动

margin-left：

左外边距，设置一个正值，元素会向右移动

 

margin也可以设置负值，如果是负值则元素会向相反方向移动

 

元素在页面中是按照自左向右的顺序排序的

所有默认情况下如果我们设置的左和上外边距则会移动元素自身

而设置下和右外边距则会移动其它元素

margin的简写属性

margin可以同设置四个方向的外边距，用法和padding一样

margin-top: 100px;

margin-bottom: 100px;

margin-left: 100px;

margin-right: 100px;

margin-bottom: -100px;

margin-top: -100px;

 

### 盒子的水平布局

 

![img](file:///C:\Users\ZWK\AppData\Local\Temp\ksohtml8696\wps1.png) 

 

 

![img](file:///C:\Users\ZWK\AppData\Local\Temp\ksohtml8696\wps2.png) 

 

 

### ==垂直外边距的折叠（重叠）==

-相邻的垂直方向外边距会发生重叠现象

==-兄弟元素==

-兄弟元素间的相邻垂直外边距会取两者之间较大值（两者都是正值）

==-特殊情况：==

如果相邻的外边距一正一负，则取两者的和

如果相邻的外边距都是负值，则取两者中绝对值较大的

-兄弟元素之间的外边距的重叠，对于开发是有利的，所有我们不需要进行处理。

==-父子元素==

-父子元素间相邻外边距，子元素的会传递给父元素（上外边距）

-父子外边距的折叠会影响到页面的布局，必须要进行处理

 

### 行内元素的盒模型

-行内元素不支持设置宽度和高度

-行内元素可以设置padding，但是垂直方向padding不会影响页面的布局

-行内元素可以设置border，垂直方向的border不会影响页面的布局

-行内元素可以设置margin，垂直方向的margin不会影响布局

 

### ==display==

> ：用来设置元素显示的类型

可选值：

​	inline将元素设置为行内元素

​	block将元素设置为块元素

​	Inline-block将元素设置为行内块元素

行内块元素：既可以设置宽度和高度又不会独占一行

table将元素设置为一个表格

none元素不在页面中显示



### ==visibility==

> 用来设置元素的显示状态

可选值：

visible默认值，元素在页面中正常显示

hidden元素在页面中隐藏 不显示，但是依然占用页面的位置



```
span{

  display: block;

  /*** visibility: visible; \*/**

  visibility: hidden;

  width: 100px;

  height: 100px;

  background-color: brown;

 }
```

 

### 默认样式

-通常情况下，浏览器都会为元素设置一些默认样式

-默认样式的存在会影响到页面的布局，通常情况下编写网页时必须要去除浏览器的默认样式（PC端的页面）

 

### 盒子的大小

默认情况下，盒子可见框的大小又内容区，内边框，边框共同决定

box-sizing 用来设置盒子尺寸的计算方式（设置width和height的作用）

可选值：

content-box默认值，宽度和高度用来设置内容区的大小

border-box宽度和高度用来设置整个盒子可见框的大小

width和height指的是内容区和可见框和边框的总大小

 

### 轮廓阴影和圆角

 

==box-shadow==用来设置元素的阴影效果，阴影不会影响页面布局

第一个值：水平偏移量 设置阴影的水平位置 正值向右移动 负值向左移动

第二个值：垂直偏移量 设置阴影的垂直位置 正值向下移动 负值向上移动

第三个值：阴影的模糊半径

第四个值：阴影的颜色

 

==outline==:用来设置元素的轮廓线，用法和border一模一样

轮廓和边框不同的点，就是轮廓不会影响到可见框的大小

 

==border-radius==:用来设置圆角 圆角设置的圆的半径大小

border-radius：可以分别指定四个角的圆角

四个值 左上 右上 右下 左下

三个值 左上 右上/左下 右下

两个值 左下/右下 右上/左下

 

------



## ==浮动==

> ：通过浮动可以使一个元素向其父元素的左侧或者右侧移动

使用==float==属性来设置子元素的浮动

可选值：

none 默认值，元素不浮动

left元素向左浮动

right元素向右浮动

 

==注意==：元素设置浮动后，水平布局的等式便不需要强制成立

元素设置浮动以后，会完成从文档流中脱离，不会占用文档流的位置

所以元素下边的还在文档流中的元素会自动向上移动

 

### 浮动时的特点

\1. 浮动元素会完全脱离文档流，不再占据文档流中的位置

\2. 设置浮动以后元素会向父元素的左侧或右侧移动

\3. 浮动元素默认不会从父元素中移出

\4. 浮动元素向左或向右移动时，不会超过它前边的其它浮动元素

\5. 如果浮动元素的上边是一个没有浮动的块元素，则浮动元素无法上移

\6. 浮动元素不会超过它上边的浮动的兄弟元素，最多最多就是和它一样高

 

浮动元素不会盖住文字，文字会自动环绕在浮动元素的周围，所以我们可以利用浮动来设置文字环绕图片的效果

元素设置为浮动后，从文档流中脱离后，元素的一些特点也会发生变化：

脱离文档流的特点：脱离文档流后，不再需要区分块元素和行内元素

==块元素：==

\1. 不再会独占页面的一行

\2. 脱离文档流以后，块元素的宽度和高度默认被内容撑开

==行内元素：==

行内元素脱离文档流以后会变成块元素，特点和块元素一样

 

### ==高度塌陷和BFC==

在浮动布局中，父元素的高度默认是被子元素撑开的

当子元素浮动后，其会完全脱离文档流，子元素从文档流脱离将会无法撑起父元素的高度，导致父元素的高度丢失

 

==父元素高度丢失后，其下的元素会自动上移，导致页面混乱==

==所以高度塌陷是浮动布局中比较常见的一个问题==

 

#### ==BFC==

> 块级格式化环境

BFC是CSS的一个隐藏的属性，可以为一个元素开启BFC

开启BFC该元素会变成一个独立的布局区域

==元素开启BFC后的特点：==

\1. 开后的元素不会被浮动元素所覆盖

\2. 其子元素不会和父元素发生外边距重叠

\3. 开后的元素可以包含浮动的子元素

==可以通过一些特殊方式开启元素的BFC==

\1. 设置元素的浮动（不推荐）

\2. 将元素设置为行内元素（不推荐）

\3. 将元素的overflow设置为一个非visible的值

-常用的方式为元素设置overflow ：hidden开启其BFC以使其可以包含浮动元素

 

**如果我们不希望某个元素因为其它元素浮动的影响而改变位置**

**可以通过clear属性来清除浮动元素对当前元素所产生的影响**

**clear：清除浮动元素对当前元素所产生的影响**

可选值：

left清除左侧浮动元素对当前元素所产生的影响

right清除右侧浮动元素对当前元素所产生的影响

both清除两边浮动元素对当前元素所产生的影响

原理：设置清除浮动后，浏览器会设置为元素添加上外边距，以使其位置不受其它元素的影响



 使用after伪类解决高度塌陷

```
.box1::after{
            content: "";
            display: block;
            clear: both;
}
```



clearfix这个样式可以同时解决高度塌陷和外边距重叠的问题

```
.clearfix::before,
.clearfix::after{
            content: "";
            display: table;
            clear: both;
}
```

 

------



## ==定位==（position）

-定位是一种更高级别的布局手段

-通过定位可以将元素摆放到页面的任意位置

-通过position属性来设置定位

可选值：

static默认值，元素是静止的没有开启定位

relative开启元素的相对定位

absolute开启元素的绝对定位

fixed开启元素的固定定位

sticky开启元素的粘滞定位

 

#### 相对定位：

当前元素的position属性值设置为relative时开启了元素的相对定位

特点：

\1. 元素开启相对定位以后，如果不设置偏移量元素不会发生任何变化

\2. 相对定位是参照于元素的文档流中的位置来定位的

\3. 相对定位会提升元素的层次

\4. 相对定位不会使元素脱离文档流

\5. 相对定位不会改变元素的性质

 

#### 偏移量：（offset）

当元素开启了定位后，可以通过偏移量来设置元素的位置

top：定位元素和定位位置上边的距离

Bottom：定位元素和定位位置下边的距离

left：定位元素和定位位置左侧的距离

right：定位元素和定位位置右侧的距离

 

#### 绝对定位

当元素的position属性设置为absolute时，则开启了元素的绝对定位

绝对定位的特点：

\1. 开启绝对定位后，如果不设置偏移量元素的位置不会发生变化

\2. 开启绝对定位后，元素会从文档流中脱离

\3. 绝对定位会改变元素的性质，行内变成块，块的宽高被内容撑开

\4. 绝对元素会使元素提升一个层级

\5. 绝对定位元素是相对于其包含块进行定位的

 

#### 包含块

> ：（containing block）

-正常情况下，

包含块就是离当前元素最近的祖先块元素

-绝对定位的包含块：

包含块就是离它最近的开启了定位的祖先元素，

如果所有的祖先元素都没有开启定位则相对于（html）根元素进行定位

 

#### 固定定位

-将元素的position属性设置为fixed则开启了元素的固定定位

-固定定位也是绝对定位的一种，所以固定定位的大部分特点都和绝对定位一样	-唯一的不同的是固定定位永远参照于浏览器的视口进行定位

固定定位的元素不会随网页的滚动条滚动

 

#### 粘滞定位

-当元素的position属性设置为sticky时则开启了元素的粘滞定位

-粘滞定位和相对定位的特点基本一致，

不同的是粘滞定位可以在元素到达某个位置时将其固定

 

#### 元素的层级

对于开启了定位的元素，可以通过z-index属性来指定元素的层级

z-index需要一个整数作为参数，值越大元素层级越高，元素层级越高越优先显示

如果元素的层级一样，则优先显示靠下的元素

 

祖先元素的层级再高也不会盖住后代元素

 

------



## 字体族

字体相关的样式：

color用来设置字体颜色

font-size字体的大小

相关单位

em相当于当前元素的一个font-size

rem相当于根元素的一个font-size

 

### font-family字体族

可选值：

serif衬线字体

sans-serif非衬线字体

monospace等宽字体

-指定字体的类别，浏览器会自动该类别下的字体

-font-family可以同时指定多个字体，多个字体间用，隔开

字体生效时优先使用第一个，如果第一个不能用，就用第二个，以此类推

 

font-face:可以将服务器中的字体直接提供给用户使用

font-family指定字体的名字

src:url()服务器中字体的路径

@font-face {

​      font-family: ;

​      src: url();

​    }

 

### 图标字体（iconfont）

-在网页中经常需要使用的一些图标，可以通过图片来引入图标

但是图片的大小本身比较大，并且非常不灵活

-所以在使用图标时，我们还可以将图标直接设置为字体

然后通过font-face的形式来对字体进行引入

-这样我们就可以通过使用字体的形式来使用图标

 

通过伪元素来设置图标字体

\1. 找到要设置图标的元素通过before或after选中

\2. 在content中设置字体的编码

\3. 设置字体的样式

fab

font-family:’font awesome 5 brands’；

 

fas

font-family:’font awesome 5 free’；

Font-weight:900;

 

### ==行高==（line height）

-行高指的是文字占有的实际高度

-可以通过line-height来设置行高

行高可以直接指定一个大小（px em）

也可以直接为其设置一个整数

如果是一个整数的话，行高将会是字体的指定的倍数

行高经常还用来设置文字的行间距

行间距=行高-字体大小

 

 

### 字体框

-字体框就是字体存在的格子，设置font-size实际上就是在设置字体框的高度

行高会在字体框的上下平均分配

 

字体的简写属性：

font可以设置字体相关的所有属性

语法：

font:字体的大小/行高 字体族

行高可以省略不写 如果不写使用默认值

 

font-weight: 字重 字体的加粗

可选值：

normal 默认值 不加粗

bold 加粗

font-style 字体的风格

normal 正常

italic 斜体

 

 

 

### ==text-align==

​	文本的水平对齐

可选值：

left左侧对齐

right右对齐

center居中对齐

justify两端对齐

 

### ==vertical-align==

​	设置元素垂直对齐的方式

可选值：

baseline默认值 基线对齐

top 顶部对齐

bottom 底部对齐

middle 居中对齐

 

### ==text-decoration== 

​	设置文本修饰

可选值：

none什么都没有

underline下划线

line-through删除线

overlie上划线



white-space

​	设置网页如何处理空白

可选值：

none正常

nowrap不换行

pre保留空白

 

------



## 背景

### background-color

​	设置背景颜色

### background-image

​	设置背景图片

-可以同时设置背景图片和背景颜色，这样背景颜色将会成为图片的背景色

-如果背景图片小于元素，则背景图片会自动在元素中平铺将元素铺满

-如果背景图片大于元素，将会一个部分背景无法完全显示

 

### background-repeat

​	用来设置背景的重复方式

可选值：

repeat默认值，背景会沿着x轴y轴双方向重复

repeat-x沿着x方向重复

repeat-y沿着y方向重复

no-repeat不重复

 

### background-position

​	用来设置背景图片的位置

设置方式：

通过top，left，bottom，center几个方位的词来设置背景图片的位置

使用方位词时必须要同时指定两个值，如果只是写一个那第二个默认为center

通过偏移量来指定背景图片的位置

水平方向的偏移量 垂直方向的偏移量

 

图片属于网页中的外部资源，外部资源都需要浏览器单独发送请求加载

浏览器加载外部资源时是按需加载的，用则加载，不用则不加载

 

### 解决图片闪烁的问题

可以将多个图片统一到一个大图片中，然后通过调整background—position来显示相应的图片

这样图片会同时加载到页面中

就可以有效的避免出现闪烁的问题

这一个应用十分广泛，被成为css-Sprite

### 雪碧图的使用步骤

\1. 先确定要使用的图标

\2. 测量图标大小

\3. 根据测量结果创建一个元素

\4. 将雪碧图设置为元素的背景图片

\5. 设置一个偏移量以显示正确的图片

 

------



## 渐变

通过渐变可以设置一些复杂的背景颜色，可以实现从一个颜色向其它颜色过渡的效果

-渐变是图片，需要通过background—image来设置

 

### 线性渐变

​	：颜色沿着一条直线发生变化

linear-gradient()

background-image: linear-gradient(red,yellow);

红色在开头，黄色在结尾，中间是过渡区域

线性渐变的开头我们可以指定一个渐变的方向

to left

to right

to bottom

to top

xxxdeg deg表示度数

turn 表示圈

background-image: linear-gradient(to right,red,yellow);

 

-渐变可以同时指定多个颜色，多个颜色默认情况下平均分布，也可以手动指定渐变的分布情况

background-image: linear-gradient(to right,red 50px,yellow 100px);

repeating-linear-gradient()

-可以平铺的线性渐变

background-image: repeating-linear-gradient(red 50px,yellow 100px);

 

### 径向渐变

radial-gradient（）径向渐变（反射性的效果）

background-image: radial-gradient(red,yellow);

 

默认情况下径向渐变的圆心形状根据元素的形状来计算的

-正方形--->圆形

-长方形--->椭圆

我们也可以手动指定径向渐变的大小

circle

ellipse

-也可以指定渐变的位置

语法：

-radial-gradient（大小 at 位置，颜色 位置，颜色 位置...）

大小：

circle圆形

ellipse椭圆

closest-side近边

closest-corner近角

farthest-side远边

farthest-corner远角

位置：

 

 

------



## 动画

动画和过渡相似，都是可以实现一些动态的效果

不同的是过渡需要在某个属性发生变化时才会触发

动画可以自动触发动态效果

 

### 关键帧

设置动画效果必须先要设置一个关键帧，关键帧规定动画执行的每一个步骤

例：

​    @keyframes test（名字） {

​      from/0%{（表示动画开始的位置，也可以使用0%）

​        margin-left: 0;

​      }

​      to/100%{ (表示动画结束的位置，也可以使用100%)

   				margin-left: 700px;

​      }

​    }

 

### 设置一个动画

==animation-name==

​	：要对当前元素生效的关键帧的名字

例：animation-name: test;

 

==animation-duration==

​	:动画的执行时间

例：animation-duration: 4s;

 

==animation-delay==

​	：动画的延时

例：animation-delay: 1s;

 

==animation-timing-function==:ease-in-out

先加速后减速运动

 

==Animation-iteration-count==

​	:动画执行的次数

可选值：

次数

Infinite:无限执行

 

==Animation-direction==

​	:指定动画运动的方向

默认值 normal  从from向to运动

reverse 反着执行

alternate 从from向to运行重复执行动画时反向执行

Alternate-reverse 先从from向to运动，返回时反向执行

 

Animation-play-state:设置动画的执行状态

running 默认值 动画执行

Paused 动画暂停

 

Animation-fill-mode:动画的填充模式

none 默认值 动画执行完毕元素回到原来的位置

Forwards 动画执行完毕后元素会停止在动画结束的位置

backwards 动画延时等待时，元素会处于开始的位置

both 结合了forwards和backwards

简写

animation: animation-name animation-duration animation-timing-function animation-delay animation-iteration-count  animation-direction  animation-fill-mode   animation-play-state;

 

##  变形

变形就是通过CSS来改变元素的形状或位置

变形不会影响到页面的布局

transform用来设置元素的变形效果

-平移

translateX()沿着x轴方向平移

translateY()沿着y轴方向平移

translateZ()沿着z轴方向平移

平移元素，百分比是相对自身计算的

Z轴平移，调整元素在Z轴的位置，正常情况就是调整元素和人眼之间的距离

距离越大，元素离人越近

Z轴的平移属于立体效果（近大远小），默认情况下网页是不支持透视的，如果需要看见这个效果，必须要设置网页的视距

html{      perspective: 800px;

​    }

 

变形原点

transform-origin: 0 0;

默认值是center

 

## 旋转

通过旋转可以使元素沿着x y 或z旋转指定的角度

transform: rotateZ(90deg);

 

 

 

对元素进行缩放

​      transform: scale(20);两个方向都缩放

​      transform: scaleX(20);X方向

​      transform: scaleY(20);Y方向

 

 

------



## less

​	是一门css的预处理语言

-css是一个css的增强版，通过less可以编写更少的代码实现更强大的样式

-在less中添加了许多的新特性，像对象的支持，对mixin的支持。。。。。

-less的语法大体上和css的一致，但是less中增添了许多对css的拓展

所以浏览器无法直接执行less代码，要执行必须先将less转换为css，然后再由浏览器执行

 

less中的单行注释，注释中的内容不会被解析到css中

但在less中写css的多行注释，那就会被编译在css中

 

变量，在变量中可以储存一个任意的值

并且可以在需要时任意的修改值

变量的语法：@变量名

```
@a:100px;

.box5{

  width: @a;

}

@b:box6;

.@{b}{

  width: @a;

}
```

当类名时要加大括号

 

变量发生重名时，优先使用近的变量，并且可以在变量声明之前使用

 

在使用less的时候

&就表示外层的父元素

例如：

```
.box1{

  &:hover{

   color: orange;

  }

}
```

在css中表示为

.box1:hover {

 color: orange;

}

 

使用类选择器时可以在选择器的后面添加一个括号，这时我们实际上就创建了一个mixins

例如：在less中

```
.p4(){

  width: 100px;

  height: 100px;

 }

.p5{

  .p4;

}
```

在css中

.p5 {

 width: 100px;

 height: 100px;

}

 

混合函数，在混合函数中可以直接设置变量

例如：在less中

```
.test(@a,@b){

  width: @a;

  height: @b;

  background-color: #bfa;

}

div{

  .test(200px,300px);

}
```

 

在css中

div {

 width: 200px;

 height: 300px;

 background-color: #bfa;

}

 

在less中所有的数值都可以直接进行运算

可以使用@import可以引入别的less

@import "style.less";

 

------



## ==flex(弹性盒，伸缩盒)==

-是css中的又一种布局手段，它主要用来代替浮动来完成网页的布局

flex可以使元素具有弹性，让元素可以跟随页面的大小的改变而改变

-弹性容器

-要使用弹性盒，必须先将一个元素设置为弹性元素

-通过display来设置弹性容器

display：flex设置为块级弹性容器

display:inline-flex设置为行内的弹性容器

-弹性元素

-弹性容器的子元素是弹性元素（弹性项）

-弹性元素可以同时是弹性容器（可以嵌套）

 

### flex-direction

​	指定容器中弹性元素的排列方式

-可选值：

row:默认值，弹性元素水平排列

row-reverse:弹性元素在容器中反向排列

column：弹性元素纵向排列

column-reverse：自下向上

主轴：

弹性元素的排列方向称为主轴

侧轴

与主轴垂直方向的称为侧轴

 

弹性元素的属性

flex-grow指定弹性元素的伸展的系数

-当父元素有多余的空间，子元素如何伸展

-父元素的剩余空间，会按照比例进行分配

flex-shrink指定弹性元素的收缩系数

-当父元素中的空间不足以容纳所以的子元素时，如果对子元素进行收缩

 

### flex-wrap

-设置弹性元素是否在弹性容器中自动换行

可选值：

nowrap:默认值，元素不会自动换行

wrap:元素沿着辅轴方向自动换行

wrap-reverse:元素沿着辅轴反方向换行

 

flex-flow:wrap和direction的简写属性

flex-flow:row wrap;

 

### justify-content

-如何分配主轴上的空白空间（主轴上的元素如何排列）

可选值：

sflex-start：元素沿着主轴起边排列

flex-end：元素沿着主轴终边排列

center：元素居中排列

space-around：空白分布到元素两侧

space-between：空白均匀分布到元素间

space-evenly：空白分布到元素的单侧

 

### align-items

-元素在辅轴上如何对齐

-元素间的关系

-可选值：

stretch默认值，将元素的长度设置为相同的值

flex-start元素不会拉伸，沿着辅轴的起边对齐

flex-end沿着辅轴的终边对齐

center居中对齐

baseline基线对齐

 

align-content设置辅轴上的空白空间（辅轴上的元素如何排列）

可选值和justify-content相同

 

align-self:用来覆盖当前弹性元素上的align-items

 

 

弹性元素的样式

元素的增长系数

flex-grow

元素的缩减系数

flex-shrink

元素基础长度

flex-basis：指定的是元素在主轴上的基础长度

如果主轴是横向的那就是元素的宽度

如果是纵向的那就是元素的高度

-默认值是auto，表示参考元素自身的高度或宽度

-如果传递了一个具体的数值，则以该为准

 

这三个样式的简写属性：可用flex来简写

flex	增长 缩减 基础；

initial   “flex:0 1 auto”

Auto     “flex:1 1 auto”

None     “flex: 0 0 auto”

 

order:决定弹性元素的排列顺序

给每个元素指定，按等级排序，order值小等级就高

 

------

 

在移动端开发时，就不能再使用px来进行布局了

vw表示的是视口的宽度

-100vw表示一个视口的宽度

这个单位永远相当于视口宽度进行计算

 

 

## 响应式布局

-网页可以根据不同的设备或窗口大小呈现出不同的效果

-使用响应式布局，可以使一个网页适用于所以设备

-响应式布局的关键就是媒体查询

-通过媒体查询，可以为不同的设备，或设备不同的状态来分别设置样式

 

### 使用媒体查询

-语法：@media 查询规则{}

-媒体类型：

all 所有设备

print 打印设备

screen 带屏幕的设备

speech 屏幕阅读器

-可以使用，连接多个类型，这样它们之间就是一个或的关系

 

可以在媒体类型前加一个only，表示只有

only的使用主要是兼容一些老的浏览器

 

媒体特性：

width:视口的宽度

height：视口的高度

 

-min-width 视口的最小宽度

max-width 视口的最大宽度

 

<style>

  @media(min-width:500px){

​    body{

​      background-color: #bfa;

​    }

  }

</style>

 

 

样式切换的分界点，我们称其为断点，也就是网页的样式会在这个点时发生变化。

一般比较常用的断点

-小于768 超小屏幕 max-width=768px

-大于768 小屏幕  min-width=768px

-大于992 中型屏幕 min-width=992px

-大于1200 大屏幕  min-width=1200px

 

  @media only screen and (min-width:500px) and (max-width:700px){

​    body{

​      background-color: #bfa;

​    }

