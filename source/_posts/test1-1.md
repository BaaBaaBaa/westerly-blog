---
title: test1-1
tags: css
categories: test1
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
