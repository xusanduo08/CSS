### CSS居中

* 居中元素宽高确定

定位+margin负值

```html
<style>
    .container{
        width: 200px;
        height: 200px;
        border: 1px solid #000;  
        position: relative;
    }
    .item{
        position: absolute;
        width: 50px;
        height: 50px;
        background: blue;
    }
    .size{
        top: 50%;
        left: 50%;
        margin-left: -25px;
        margin-top: -25px;
    }
</style>
<div class="container">
   <div class="item size"></div>
</div>
```

定位+margin：auto+top/left/right/bottom=0

```
<style>
	.container{/*容器样式同上*/}
	.item{/*item样式同上*/}
	.size{
        top:0;left:0;right:0;bottom:0;
        margin:auto;
	}
</style>
<div class="container">
	<div class="item size2"></div>
</div>
```

定位+calc

```html
<style>
	.container{/*容器样式同上*/}
	.item{/*item样式同上*/}
	.size3{
        top: calc(50% - 25px);
        left: calc(50% - 25px);
	}
</style>
<div class="container">
	<div class="item size3"></div>
</div>
```

* 居中元素宽高不定

定位+transform: translate

translate(x, y)可以实现2D转换

```html
<style>
	.container{/*容器样式同上*/}
	.size4{
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
	}
</style>
<div class="container">
	<div class="size4">1233142142323</div>
</div>
```

line-height+text-align

line-height等于height可以使行内元素垂直居中，text-align:center可以使行内元素水平居中

```
<style>
.container{/*容器样式同上*/}
.wp{
   line-height: 200px;
   text-align: center;
 }
 .size5{
   display: inline-block;
   vertical-align: middle;
 }
</style>
<div class="container wp">
   <div class="size5">1233142142323</div>
 </div>
```

writing-mode+text-align

writing-mode能改变文字排版方向，同时对应的样式效果（比如换行,text-align,但是宽高不会）也会改变到对应方向

```html
<style>
	.container{/*容器样式同上*/}
    .wp2{
        writing-mode: vertical-lr;
        text-align: center;
    }
    .inner{
        writing-mode: horizontal-tb;/*writing-mode会继承，所以这地方要重新设置成水平布局*/
        display: inline-block;
        text-align: center;
        width: 100%;
    }
    .box{
        display: inline-block;
        text-align: left;
    }
</style>
<div class="container wp2">
	<div class="inner">
		<div class="box">1234567</div>
	</div>
</div>
```

表格居中

表格内容自带垂直居中的效果，只要添加一个水平居中属性就好了.

表格内容自带垂直居中的效果是因为td元素默认就有一个vertical-align:middle的属性

为了模拟td元素的效果，把容器的display设置为table-cell，同时加上vertical-align:middle。

```html
<style>
 .container{/*容器样式同上*/}
 .wp3{
   display: table-cell;
   vertical-align: middle;
   text-align: center;
 }
  .box2{
    display: inline-block;
  }
</style>
<div class="container wp3">
	<div class="box2">12345678</div>
</div>
```

flex：justify-content+align-items

```html
<style>
	.wp4{
        display: flex;
        justify-content: center;
        align-items: center;
	}
</style>
<div class="container wp4">
	<div class="box3">1234566</div>
</div>
```

grid布局

```html
<style>
    .wp5{
    	display: grid;
    }
    .box4{
        align-self: center;
        justify-self: center;
    }
</style>
<div class="container wp5">
	<div class="box4">jsjfklskf</div>
</div>
```

总结一下：共9种

* 定位+负margin
* 定位+margin：auto+left/right/top/bottom=0
* 定位+calc
* 定位+transform：translate
* line-height+text-align
* writing-mode+text-align
* 表格居中：display:table-cell;vertical-align: middle;text-align: center;
* flex：justify-content+align-items
* gird：align-self+justify-self