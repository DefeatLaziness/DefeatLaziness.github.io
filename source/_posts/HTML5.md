---
title: HTML5
date: 2022-07-19 15:47:05
summary: HTML5
tags: [H5]
categories: [H5]
---

# HTML5

## 1. 语义化标签

![image-20221019121404964](/Users/kim/Library/Application Support/typora-user-images/image-20221019121404964.png)

## 2. 增强型表单

![image-20221019145821805](/Users/kim/Library/Application Support/typora-user-images/image-20221019145821805.png)

![image-20221019150908650](/Users/kim/Library/Application Support/typora-user-images/image-20221019150908650.png)

![image-20221019150938610](/Users/kim/Library/Application Support/typora-user-images/image-20221019150938610.png)

## 3. 音视频标签

https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/video

https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/audio

## 4. Canvas绘图

https://developer.mozilla.org/zh-CN/docs/Web/API/Canvas_API

## 5. SVG绘图

https://developer.mozilla.org/zh-CN/docs/Web/SVG/Tutorial/Getting_Started

## 6. 地理定位

```javascript
var options = {
  enableHighAccuracy: true,
  timeout: 5000,
  maximumAge: 0
};

function success(pos) {
  var crd = pos.coords;

  console.log('Your current position is:');
  console.log('Latitude : ' + crd.latitude);
  console.log('Longitude: ' + crd.longitude);
  console.log('More or less ' + crd.accuracy + ' meters.');
};

function error(err) {
  console.warn('ERROR(' + err.code + '): ' + err.message);
};

navigator.geolocation.getCurrentPosition(success, error, options);

```



## 7. 拖放API

```html
<div draggable="true"></div> // div添加draggable为true才可拖动
```

![image-20221020154321887](/Users/kim/Library/Application Support/typora-user-images/image-20221020154321887.png)

## 8. WebWorker

![image-20221020160526297](/Users/kim/Library/Application Support/typora-user-images/image-20221020160526297.png)

## 9. WebStorage

![image-20221020163206435](/Users/kim/Library/Application Support/typora-user-images/image-20221020163206435.png)

## 10. WebSocket

https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket

