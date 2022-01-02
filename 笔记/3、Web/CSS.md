# 选择器

元素选择器

```css
/*直接通过标签元素进行选择
* 会选择所有标签*/
p{
 background-color:yellow;   
}
```

id选择器

```css
/*	给标签设置id后可通过id对标签进行选择*/
#nav{
    background-color:blue;
}
```

class选择器

```css
/*为标签定义类名，通过类名对标签进行选择*/
.nav{
    background-color:blue;
}
```

组合选择

```css
/*	元素选择器、id选择器、类选择器混合使用
*	可对特定标签下的标签进行选择
*	后代选择器（空格）、子选择器（>）、相邻兄弟选择器（+）、通用兄弟选择
*	器（~）
*/
div p{
    选择div元素内的所有p元素
}
div > p{
    选择父元素为div的所有p元素
}
div + p{
    选择在div之后的p标签
}
p ~ ul{
    选择前面有p元素的所有ul元素
}
```

# 颜色

背景颜色

```css
body {
	background-color: lightblue;	定义背景颜色
	opacity: 0.3;					定义不透明度
    background: rgba(0, 128, 0, 0.3) 
        /* 30% 不透明度的绿色背景 */
}
```

背景图片

```css
body{
    background-image:url("aaa.gif");	定义背景图片
    background-repeat: repeat-x;	定义背景图片重复次数和方向
    background-attachment: fixed;	定义背景图片是否附着
}
```

背景属性简写

```css
body {
  background: #ffffff url("tree.png") no-repeat right top;
}
在使用简写属性时，属性值的顺序为：
    background-color
    background-image
    background-repeat
    background-attachment
    background-position
```

# 边框

边框属性及样式

```css
border-style 属性指定要显示的边框类型。
    允许以下值：
    - dotted - 定义点线边框
    - dashed - 定义虚线边框
    - solid - 定义实线边框
    - double - 定义双边框
    - groove - 定义 3D 坡口边框。效果取决于 border-color 值
    - ridge - 定义 3D 脊线边框。效果取决于 border-color 值
    - inset - 定义 3D inset 边框。效果取决于 border-color 值
    - outset - 定义 3D outset 边框。效果取决于 border-color 值
    - none - 定义无边框
    - hidden - 定义隐藏边框
border-style 属性可以设置一到四个值（用于上边框、右边框、下边框和左边框）。
```

![image-20210303110037247](C:\Users\26283\AppData\Roaming\Typora\typora-user-images\image-20210303110037247.png)

边框属性

```css
body{
    border-style:solid;
    border-top-style: double;
	border-left-style: dashed;
	border-right-style: dashed;
	border-bottom-style: double;
    border-width: 5px;	边框宽度
    border-color: red;  边框颜色
}


属性简写顺序：
	- border-width
	- border-style
	- border-color
```

圆角边框

```css
body{
    border-style:solid;
    border-radius:5px;
}
```

| 属性             | 描述                               |
| :--------------- | :--------------------------------- |
| border           | 简写属性（可设置所有边框属性）     |
| border-color     | 边框的颜色                         |
| border-radius    | 圆角属性（左上、右上、右下、左下） |
| border-style     | 边框样式                           |
| border-width     | 边框宽度                           |
| border-bottom    | 下边框属性                         |
| border-left      | 左边框属性                         |
| border-right     | 右边框属性                         |
| border-top       | 上边框属性                         |
| border-top-color | 上边框颜色                         |
| border-style     | 上边框样式                         |
| border-width     | 上边框宽度                         |

简写顺序

```css
{
    border:5px solid red;
	   	 width style color
    border-color:red green blue #fff;
    			  上   右    下    左
}
```

边框外边距与内边距

```css
{
	margin: 8%;
    简写顺序rop right bottom left
    外边距会自动合并
}
{
    padding: 10px;
    
}
```

# 文本格式控制

```css
文本字体
{
    font-family:"Times New Roman";
}
文本颜色/背景色
{
    color:red;
    background-color:green;
}
文本水平对齐
{
    text-align:center;
    text-align:left;
    text-align:right;
}
文本垂直对齐
{
    vertical-align:top;
    vertical-align:middle;
    vertical-align:bottom;
}
文本方向
{
    direction:rtl;
    unicode-bidi:bidi-override;
}
文本装饰
{
    text-decoration:none;
    text-decoration:overline;
    text-decoration:line-through;
    text-decoration:underline;
}
文本字母转换
{
    text-transform:uppercase;
    text-transform:lowercase;
    text-transform:capitalize;
}
首行缩进
{
    text-indent:50px;
}
字符之间间距
{
    letter-spacing:3px;
    letter-spacing:-3px;
}
行间距
{
    line-height:0.8;
}
字间距
{
    word-spacing:10px;
}
文字阴影
{
    text-shadow:2px 2px;
}
```

连接样式

```css
根据链接处于什么状态来设置链接的不同样式
    - a:link - 正常的，未访问的链接
    - a:visited - 用户访问过的链接
    - a:hover - 用户将鼠标悬停在链接上时
    - a:active - 链接被点击时
```

列表样式

```css
{
    list-style-type:circle(square)(upper-roman)(lower-alpha);设置不同的列表标记
    list-style-image:url('page.gif');
    list-style-position:outside(inside);项目符号点位置
    list-style-type:none;删除默认设置
    margin:0;
    padding:0;
    
    属性简写
	list-style:type position image;
}
```

表格

```css
{
    border:1px solid black;设置表格边框简写（宽度 样式 颜色）
    width:100%;设置表格铺满整个屏幕
    border-collapse:collapse;将表格边框折叠为单一边框
}

tr:hover{
    可设置鼠标悬停时的样式
}
tr:nth-child(even){
    设置偶数格选中效果
}

表格外添加div标签并设置overflow-x:auto属性添加表格响应
```