---
title: css面试题
tags: css
categories: css
cover: '/img/1156990.jpg'
top_img: '/img/1057805.png'

---
## 介绍一下CSS的盒子模型

> CSS的盒子模型有哪些：标准盒子模型、IE盒子模型
> CSS的盒子模型区别：
>   标准盒子模型：margin、border、padding、content
>   IE盒子模型 ：margin、content（ border +  padding  + content ）
> 通过CSS如何转换盒子模型：
>   box-sizing: content-box;  /*标准盒子模型*/
>   box-sizing: border-box;   /*IE盒子模型*/

## img标签的title和alt有什么区别？

区别一：
  title ： 鼠标移入到图片显示的值
  alt   ： 图片无法加载时显示的值
区别二：
  在seo的层面上，爬虫抓取不到图片的内容，所以前端在写img标签的时候为了增加seo效果要加入alt属性来描述这张图是什么内容或者关键词。

## 图片格式

png:无损压缩，尺寸体积要比jpg/jpeg的大，适合做小图标。
jpg:采用压缩算法，有一点失真，比png体积要小，适合做中大图片。
gif:一般是做动图的。
webp：同时支持有损或者无损压缩，相同质量的图片，webp具有更小的体积。兼容性不是特别好。

apng: 可以替代gif，向下兼容。

## BFC

BFC就是页面上一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。

1. 了解BFC ： 块级格式化上下文。
2. BFC的原则：如果一个元素具有BFC，那么内部元素再怎么弄，都不会影响到外面的元素。
3. 如何触发BFC：
   float的值非none
   overflow的值非visible
   display的值为：inline-block、table-cell...
   position的值为:absoute、fixed

## 清除浮动有哪些方式？

  1. 触发BFC
  2. 多创建一个盒子，添加样式：clear: both;
  3. after方式
    ul:after{
    ​		content: '';
    ​		display: block;
    ​		clear: both;
    }

## ios系统中元素被触摸时产生的半透明灰色遮罩怎么去掉

<style>
	a,button,input,textarea{
		-webkit-tap-highlight-color: rgba(0,0,0,0);
	}
</style>

## webkit表单输入框placeholder的颜色值能改变吗

<style type="text/css">
	input::-webkit-input-placeholder{
		color:red;
	}
</style>

## 禁止ios长按时触发系统的菜单，禁止ios&android长按时下载图片

```css
html,body{
	touch-callout: none;
	-webkit-touch-callout: none;
	
	user-select:none;
	-webkit-user-select:none;
}
```

## 禁止ios和android用户选中文字

```
html,body{
	user-select:none;
	-webkit-user-select:none;
}
```

## CSS的引入方式有哪些？link和@import的区别是什么？

> CSS有3种引入方式。
>
> - 行内式是指将样式写在元素的 style属性内。
> - 内嵌式是指将样式写在 style元素内。
> - 外链式是指通过link标签，引入CSS文件内的样式。
>
> 通过link标签引入样式与通过@ import方法引入样式有如下区别。
>
> （1）加载资源的限制。
>
> link是 XHTML的标签，除了加载CSS文件外，还可以加载RSS等其他事务，如加载模板等。
>
> @ import只能加载CSS文件。
>
> （2）加载方式。
>
> 如果用link引用CSS，在页面载入时同时加载，即同步加载。
>
> 如果用@ import引用CSS，则需要等到网页完全载入后，再加载CSS文件，即异步加载。
>
> （3）兼容性。
>
> link是 XHTML的标签，没有兼容问题。
>
> @ import是在CSS2.1中提出的，不支持低版本的浏览器。
>
> （4）改变样式
>
> link的标签是DOM元素，支持使用 JavaScript控制DOM和修改样式；@ import是种方法，不支持控制DOM和修改样式。

## CSS优先级如何排序？

![1693760839916](C:\Users\刘好鸭儿廋先生\AppData\Roaming\Typora\typora-user-images\1693760839916.png)

## 用CSS画一个三角形

```
border-left:100px solid transparent;
border-right:100px solid transparent;
border-top:100px solid transparent;
border-bottom:100px solid #ccc;
```

