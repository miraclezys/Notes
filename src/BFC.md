# BFC
> [细说CSS中的BFC](https://juejin.im/post/583bb606a22b9d006c141286)
> [理解CSS中BFC](https://www.w3cplus.com/css/understanding-block-formatting-contexts-in-css.html)
> [10 分钟理解 BFC 原理](https://zhuanlan.zhihu.com/p/25321647)

BFC(Block formatting Context)，直译为“块级格式化上下文”。
BFC是W3C CSS2.1规范中的一个概念，它决定了如何对其内容进行定位，以及与其他元素的关系和相互作用。可以将BFC理解为一个独立的容器，容器里面的元素不会再布局上影响到外面的元素，并且BFC具有普通容器没有的一些特性。

## 触发BFC
* float的值不为none
* position的值不为sattic或者relative
* display的值为table-cell, table-caption, inline-block, flex或inline-flex中的其中一种
* overflow的值不为visible

## BFC的特性和应用
1. 同一个BFC下外边距会发生折叠

   ```html
   <style>
     div {
       width: 200px;
       height: 200px;
       background-color: lightblue;
       margin: 100px;
     }
   </style>

   <body>
     <div></div>
     <div></div>
   </body>
   ```

   从上图可以看出，两个div元素处于同一个BFC容器下(这里指body)，所以两个div元素的外边距发生了重叠，即两个div的间距是100px，不是200px

   ​