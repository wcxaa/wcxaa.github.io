---
title: transform 影响position：absolute/fixed的后代元素 [翻译]
date: 2017-05-25
---

参考自 <http://meyerweb.com/eric/thoughts/2011/09/12/un-fixing-fixed-elements-with-css-transforms/>

根据[最新的CSS 2D Transforms 草稿的介绍](http://www.w3.org/TR/2009/WD-css3-2d-transforms-20091201/#introduction)（原文发表时最新）,一个transformed的元素会创建一个containing block ，作用于它所有应用了position的后代元素，这在transformed元素没有position设置时发生。

我们先看看这个demo:

<p data-height="500" data-theme-id="dark" data-slug-hash="YVMGNx" data-default-tab="html,result" data-user="wcx" data-embed-version="2" data-pen-title="demo-1" class="codepen">See the Pen <a href="https://codepen.io/wcx/pen/YVMGNx/">demo-1</a> by wcx (<a href="https://codepen.io/wcx">@wcx</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

一个`position:static`的div,有一个`position: absolute`的span后代，span 的 containing block 是根元素，就是span根据 根元素 来绝对定位，这符合我们的预期。

但是一旦你给div加了 `transform: rotate(0deg)` 属性，那div就变成span的containing  block了，不是根元素了。这种情况有点不符合我们预期，但还算能理解。

---

更不符合预期的是下面这种情况，看这个demo:

<p data-height="500" data-theme-id="dark" data-slug-hash="gWEBPN" data-default-tab="css,result" data-user="wcx" data-embed-version="2" data-pen-title="demo-2" class="codepen">See the Pen <a href="https://codepen.io/wcx/pen/gWEBPN/">demo-2</a> by wcx (<a href="https://codepen.io/wcx">@wcx</a>) on <a href="https://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

如果span设置了`position: fixed`，并且div应用了`transform: rotate(0deg)`, 那么span将不是根据viewport来固定定位了，而是会根据div来定位，这相当于span的`position: fixed` 被改为 `position: absolute`，并且页面滚动时，'固定定位'的span会跟着一起滚动，这不符合我们的直觉。

>     //wcx:这篇文章大概就说了这么多，我也是简短翻译，说明主要意思