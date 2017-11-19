---
title: 基于js+rem的移动端自适应方案小总结
date: 2017-09-03 23:48:20
tags: [CSS, Less]
categories:
 - 实践
---
## 用rem作单位
rem是CSS3引进的一个新的单位。W3C关于rem的定义是：等于根元素上的font-size的计算值。比如，当html的font-size为16px时，1rem=16px。
它的兼容性也较高，支持的浏览器有iOS Safari9.3+、Android Browser4.4+、Chrome49+、Firefox52+、IE11等，符合一般项目的浏览器兼容要求。
<!-- more -->
![适配](/static/2017/09/rem.jpeg)

根据rem的特性，我们可以想到，我们能在设置元素的width、height等属性时使用rem作为单位，用js根据屏幕的分辨率，改变根元素html的font-size的值，从而实现元素相对于屏幕等比缩放。

## 用less写单位转换
如果设置每一个元素的属性时都需要手动计算rem就会很麻烦，于是我们可以用less写一个类pxtorem
``` less
@base: 50px;
html {
    font-size: @base;
}

.pxtorem(@pro, @px) {
    @{pro}: (@px / @base) * 1rem;
}
/*较特殊的calc函数*/
.pxtoremcalc(@pro, @per, @px) {
    @{pro}: calc(~"@{per} - "(@px / @base) * 1rem);
}
```
调用
``` less
.example {
    .pxtorem(width, 120px);
    .pxtorem(height, 100%, 50px);
}
```

或者也可以用sass写一个pxtorem函数。因为less不支持函数，所以如果用less需要先安装插件[less-plugin-functions](https://github.com/seven-phases-max/less-plugin-functions)。详细用法看链接的readme.md

## 用js改变根元素的font-size值
``` js
function setFont(){
    var baseWidth = 800; // 设计稿宽度
    var baseHeight = 1232; // 设计稿高度
    var htmlFont = 50; // 预设的根元素html的font-size值
    var scale = Math.min(window.innerWidth / baseWidth, window.innerHeight / baseHeight);
    document.getElementsByTagName('html')[0].style.fontSize = scale * htmlFont + 'px';
}
window.addEventListener('resize',setFont,false)
setFont();
```
其中，800px是设计稿的宽度，50px是预设的html的font-size值。设置50px是因为减少小数位，以及方便通过rem单位的值看出px单位的值，比如，0.12rem即为24px。之后，判断屏幕宽高与设计稿宽高的比，取相差较大的那个进行缩放。比如，屏幕宽度为320时，html的font-size就会设置为320 / 800 * 50 + ‘px’ 即20px。

## 最后
在此方案中，可灵活使用rem单位，对于不希望等比缩放的属性如font-size可使用px为单位。而对于图片，针对retina屏幕引起的问题，常见的解决方案有根据设备的dpr用图片服务器生成1x、2x、3x的图片等，这里暂不做探究。
