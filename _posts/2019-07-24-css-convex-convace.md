---
layout:     post
title:      "CSS 凹凸弧形"
subtitle:   "CSS 凹凸弧形实现"
date:       2019-07-24 22:57:54
author:     "pengcu"
header-img: "img/home-bg-o.jpg"
catalog: true
tags:
    - CSS
---
> 只是简单记录了下 css 实现的弧形

<!--more-->

```css
.convex {
    width: 100%;
    height: 300px;
    position: relative;
    overflow: hidden;
}

.convex::after {
    content: '';
    width: 100%;
    height: 300px;
    position: absolute;
    border-radius: 0 0 80% 80%;
    background: linear-gradient(to right, #f1b714, #f6fae0);
}

.concave {
    width: 100%;
    height: 300px;
    position: relative;
    overflow: hidden;
    background: linear-gradient(to right, #f1b714, #f6fae0);
}

.concave::after {
    content: '';
    position: absolute;
    width: 100%;
    height: 150px;
    bottom: 0;
    border-radius: 60% 60% 0 0;
    background: #fff;
}
```
```html
<div class="convex"></div>
<div class="concave"></div>
```

效果如图:
![example](/img/css/convex.png)

凹 和 凸 的实现并不一样，凹是用 `:after` 伪元素根据背景色遮盖的实现。临时想出的方法，并不一定合适。