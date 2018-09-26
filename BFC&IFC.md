### BFC和IFC

#### 普通流（normal flow）

处于普通流中的盒子都会属于一个格式化上下文（Formatting context），要么是块级格式化上下文（Block formatting context，BFC），要么是行内格式化上下文（Inline formatting context，IFC），但不会两者都是。__块级盒子参与的是BFC，行内盒子参与的是IFC__。

#### `position schemes`：定位策略

一个盒子可以根据三种定位策略来渲染：

* `normal flow`：普通流。在css2中，普通流包括处于BFC中的块级盒子、处于IFC中的行内盒子，还有相对定位的块级和行内盒子。
* `floats`：在浮动模式下，盒子首先会按照普通流来渲染，然后跳出普通流并尽可能的左右移动。
* `absolute position`：在绝对定位模式下，盒子会完全跳出普通流，它的位置会相对于有`position`属性的包含容器来定位（assigned a position width respect to a containing block），此时，绝对定位的盒子对兄弟盒子的布局没有影响。

如果一个元素有浮动属性或者绝对定位属性，或者是一个根元素，那么我们就说这个元素是脱离普通流的（out of flow），而剩下的都叫处于普通流中（in-flow）

#### 通过`position`属性来选择定位策略

`position`属性有5个值：`static`、`relative`、`absolute`、`fixed`、`inherit`

* `static`、`relative`：此时盒子都会处于普通流中。`static`为`position`属性的默认值。`position:relative`的盒子，它的位置会相对于自身处于正常模式下的位置根据`top`、`left`、`bottom`、`right`属性进行移动。
* `absolute`、`fixed`：`fixed`可以说是`absolute`的子类，两者都会是使元素脱离普通流。`fixed`元素的定位会根据`absolute`来计算，只不过，`fixed`的盒子会有一些参照，比如视口（viewport），此时，盒子不会因为鼠标的滚动而滚动。`absolute`定位下的盒子不会发生外边距合并。

#### BFC和IFC

##### Block formatting context：BFC

什么情况下会产生新的BFC：

* float不为none
* position不为static、relative
* display为inline-block、table-cell、table-caption
* overflow不为visible

在以上条件下的盒子都会为自己的内容创建一个新的BFC。

在BFC中，盒子都是从包含容器的顶部开始，从上至下一个接一个的垂直排列。两个盒子之间的垂直距离由margin属性来决定。在同一个BFC中，相邻的块级盒子之间的margin会发生折叠。

BFC更多的布局规则如下：

* 每个元素的左边，与包含的盒子的左边相接触，即使浮动也是如此（如果是右布局的话则靠近右边缘）；
* BFC的区域不会与float重叠；
* BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素，反之也是如此；
* 计算BFC的高度时，浮动元素也参与其中。

###### BFC应用

比如，我们想实现一个简单的两栏布局，如下图所示：

![Alt text](img/20170212.png)

通常我们都是这么实现的

```html
<style>
	.black{
      	width:200px;
      	height:200px;
      	float:left;
		background:black;
	}
	.red{
      	width:200px;
      	height:300px;
      	margin-left:200px;
		background:red;
	}
</style>
<div class="black"></div>
<div class="red"></div>
```

这样确实也能实现如图的效果，但这种实现利用了左浮动的盒子的宽度和margin-left。如何使代码适用于更普遍的情况？答案就是使用`overflow:hidden`为右边的盒子重新创建一个BFC。

```html
<style>
	.black{
      	width:200px;
      	height:200px;
      	float:left;
		background:black;
	}
	.red{
      	width:200px;
      	height:300px;
      	overflow:hidden;
		background:red;
	}
</style>
<div class="black"></div>
<div class="red"></div>
```

上面的代码渲染出来的效果和上图一致，而且具有响应式的效果。利用到的就是BFC的布局规则之一：BFC的区域不会与float重叠。

BFC还可以用来解决外边距合并问题。

```
<style>
	.top{
      	width:100px;
      	height:100px;
      	background:red;
      	margin:20px;
	}
	.bottom{
      	width:100px;
      	height:100px;
      	background:block;
      	margin:20px;
	}
</style>
<div class="top"></div>
<div class="bottom"></div>
```

上面代码显示的效果如下：

![Alt text](img/201702121700.png)

可以看到，上下两个盒子之间的边距只有20px，而不是想象中的40px，这就是因为发生了外边距合并（外边距合并只发生在垂直方向，水平方向不会有外边距合并），解决方法是使两个div 处于不同的BFC中，为其中一个元素添加具有BFC的包裹容器（[Margins of elements that establish new block formatting contexts (such as floats and elements with ['overflow'](https://www.w3.org/TR/CSS21/visufx.html#propdef-overflow) other than 'visible') do not collapse with their in-flow children.](https://www.w3.org/TR/CSS21/box.html#collapsing-margins)）。修改代码如下：

```html
<style>
	.top{
      	width:100px;
      	height:100px;
      	background:red;
      	margin:20px;
	}
	.bottom{
      	width:100px;
      	height:100px;
      	background:block;
      	margin:20px;
	}
	.box{
      	overflow:hidden;
	}
</style>
<div class="top"></div>
<div class="box">
	<div class="bottom"></div>
</div>
```

显示效果如下：

![Alt text](./img/201702121712.png)

BFC清除浮动。

```
<style>
	.parent{
      	width: 500px;
      	border:1ps solid black;
	}
	.black{
		margin-top:10px;
      	width:200px;
      	height:300px;
      	float:left;
		background:black;
	}
	.red{
		margin-top:10px;
      	width:200px;
      	height:300px;
      	float:left;
		background:red;
	}
</style>
<div>
	<div class="balck"></div>
	<div class="red"></div>
</div>
```

由于子元素为浮动元素，处在另一个BFC中，所以父元素也就无法被撑开（BFC之间相互不影响），解决方法就是为父元素加上`overflow:hidden`(计算BFC的高度时，浮动元素也参与其中)。显示效果如下：

![Alt text](./img/201702121736.png)



##### Inline formatting context：IFC

在IFC中，盒子都是从包含从起开始，水平的一个接一个的格式化。水平方向的margin、border、padding控制着盒子相互之间的距离。



http://blog.sina.com.cn/s/blog_877284510101jo5d.html

