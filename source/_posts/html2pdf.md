---
title: JS将HTML导出PDF
date: 2017-09-03 23:44:44
tags: [Canvas, Javascript]
categories:
 - 实践
---
## 依赖包
基于jsPDF和html2canvas两个包
```
npm install jspdf –save-dev
npm install html2canvas –save-dev
```
 - jspdf github地址：https://github.com/MrRio/jsPDF
 - html2canvas github地址：https://github.com/niklasvh/html2canvas

<!-- more -->
## 基本思路
 1. 算出页数和每一页的宽度和高度
 2. 创建一个jsPDF实例。循环，调用html2canvas方法，渲染每一页page为canvas，并获取图片数据插入pdf中。插入之后，调用jsPDF.addPage()方法，加一页。然后，改变dom的css属性top，使其向上移动一页的距离，进入下一页。
 3. 当到达最后一页，获取最后一页的图片数据之后调用jsPDF.save()方法，保存pdf文件。


``` js
import jsPDF from 'jspdf'
import html2canvas from 'html2canvas'

/**
 * @Author   seminelee
 * @DateTime 2017-09-01
 * @param    {object}   dom          dom
 * @param    {number}   domWidth     dom宽度
 * @param    {number}   domHeight    dom高度
 * @param    {number}   windowWidth  window.innerWidth
 * @param    {number}   windowHeight window.innerHeight
 * @param    {string}   fileName     导出文件名
 * @return   {function}              [description]
 */

const html2pdf = function(dom, domWidth, domHeight, windowWidth, windowHeight, fileName){

    const pageNum = Math.ceil(domHeight / windowHeight);

    const a4Width = 592.28;
    const a4Height = 841.89;
    const imgWidth = domWidth / windowWidth * a4Width;
    const imgHeight = domHeight / windowHeight * a4Height;

    let pdf = new jsPDF('', 'pt', 'a4');

    for ( let i= 0; i < pageNum; i++) {
        dom.style.top = (- i * windowHeight) + 'px';
        html2canvas(dom, {
            onrendered: (canvas) => {
                let pageData = canvas.toDataURL('image/jpeg', 1.0);

                pdf.addImage(pageData, 'JPEG', 0, 0, imgWidth, imgHeight );

                if (i == pageNum -1) {
                    pdf.save(fileName)
                } else {
                    pdf.addPage();
                }
            }
        })
    }
    dom.style.top = 0;
}

export default html2pdf;
```
