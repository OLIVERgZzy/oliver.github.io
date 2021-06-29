---
title: 微信小程序中使用正则表达式替换富文本中图片的宽度
top: false
cover:
toc: false
mathjax: false
date: 2021-03-09 16:27:02
top_img:
description: 在开发微信小程序时，富文本渲染时图片
tags:
  - JavaScript
categories:
  - 前端
keywords: 前端, javascript, 正则表达式, 正则, 正则替换图片宽度
---

```ts
const formatRichText = (html: string) => {
  if (!html || html.length === 0) return "";
  let showW: number, showH: number;
  let clientWidth = 375;

  if (clientWidth > 750) {
    clientWidth = 750;
  }
  const rem = (clientWidth / 375) * 100;
  showW = clientWidth - rem * 0.2;

  const newContent = html.replace(/<img[^>]*>/gi, function (match) {
    const getW: any = match.match(/width\b\s*=\s*[\'\"]?([^\'\"]*)[\'\"]?/i); //获取到的宽
    let getH: any = match.match(/height\b\s*=\s*[\'\"]?([^\'\"]*)[\'\"]?/i); //获取到的高
    const getDataW: any = match.match(
      /data\-width\b\s*=\s*[\'\"]?([^\'\"]*)[\'\"]?/i
    ); //获取到的宽--兼容版本
    const getDataH: any = match.match(
      /data\-height\b\s*=\s*[\'\"]?([^\'\"]*)[\'\"]?/i
    ); //获取到的高--兼容版本

    // 兼容
    if (getDataW) {
      if (parseFloat(getDataW[1]) >= 90) {
        //大于300的
        showH = parseInt((parseFloat(getDataH[1] + "") * showW) / 90 + "");
        const replaceImg = match.replace(
          /style=\"(.*)\"/gi,
          'style="width:' + showW + "px;height:" + showH + '"'
        );
        match =
          '<div style="width:' +
          showW +
          "px;height:" +
          showH +
          'px">' +
          replaceImg +
          "</div>";
      } else {
        // <300
        showW = parseInt(parseFloat(getDataW[1] + "") * 3 + "");
        showH = parseInt(parseFloat(getDataH[1] + "") * 3 + "");
        const replaceImg = match.replace(
          /style=\"(.*)\"/gi,
          'style="width:' + showW + "px;height:" + showH + 'px"'
        );
        match =
          '<div style="width:' +
          showW +
          "px;height:" +
          showH +
          'px">' +
          replaceImg +
          "</div>";
      }
    } else if (getW) {
      // 新的
      if (getW > showW) {
        if (getH) {
          showH = (showW * parseInt(getH[1])) / parseInt(getW[1]);
          showH = showH + "px";
        } else {
          showH = "auto";
        }
        const replaceImg = match.replace(
          /style=\"(.*)\"/gi,
          'style="width:' + showW + "px;height: " + showH + '"'
        );
        match =
          '<div style="width:' +
          showW +
          "px;height: " +
          showH +
          '">' +
          replaceImg +
          "</div>";
      } else {
        if (getH) {
          getH = getH[1] + "px";
        } else {
          getH = "auto";
        }
        const replaceImg = match.replace(
          /style=\"(.*)\"/gi,
          'style="width:' + getW[1] + "px;height: " + getH + '"'
        );
        match =
          '<div style="width:' +
          getW[1] +
          "px;height: " +
          getH +
          '">' +
          replaceImg +
          "</div>";
      }
    }
    return match;
  });
  return newContent;
};
```
