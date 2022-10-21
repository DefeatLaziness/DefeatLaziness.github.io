---
title: HTML5
date: 2022-07-19 15:47:05
summary: HTML5
tags: [H5]
categories: [H5]
---

# HTML5

## 1. 语义化标签

![](https://s2.loli.net/2022/10/21/PsZYNSTvpmywQLi.png)



## 2. 增强型表单

![](https://s2.loli.net/2022/10/21/UOwVYl2eSRqDcPX.png)

![](https://s2.loli.net/2022/10/21/hyEloatJQ4Sn5uO.png)

![](https://s2.loli.net/2022/10/21/mMJ8qaVuZRyEhIG.png)

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

![](https://s2.loli.net/2022/10/21/uwND1vGztZJPVgH.png)

## 8. WebWorker

![](https://s2.loli.net/2022/10/21/SIZtqa5U89FEmcH.png)

## 9. WebStorage

![](https://s2.loli.net/2022/10/21/sWjtfqz3OplG2m1.png)

## 10. WebSocket

https://developer.mozilla.org/zh-CN/docs/Web/API/WebSocket

