1.querySelector 获取指定元素中匹配css选择器的元素：
```js
// 作用在document
document.querySelector("#nav"); // 获取文档中id="nav"的元素
document.querySelector(".nav"); // 获取文档中class="nav"的元素
document.querySelector("#nav li:first-child"); // 获取文档中id="nav"下面的第一个li元素

// 也可以作用在其他元素
let nav = dodocument.querySelector("#nav");
nav.querySelector("li"); // 如果有多个li的话，返回第一个li

```

注意：无论如何只返回一个元素，如果有多个素，那也只返回第一个✅

2. querySelectorAll 获取指定元素中匹配css选择器的所有元素：

```js
let list = document.querySelectorAll("li");  // NodeList(2) [li, li] 这里假设返回2个

```

注意：返回的值是一个类数组，可以使用forEach（有些浏览器无法使用，建议还是转一下），但是无法使用filter、map等，需要转换一下：

```js
Array.from(list).map();

```

3. closest 跟querySelector相反，该元素可以向上查询，也就是可以查询到父元素：

```js
document.querySelector("li").closest("#nav");

```

4. dataset 就跟原生微信小程序一样，能获取标签上以"data-"为前缀的属性集合：

```html
<p data-name="蜘蛛侠" data-age="16"></p>
```
```js
document.querySelector("p").dataset; // {name: "蜘蛛侠", age: "16"}

```

注意：虽然可以用getAttribute方法获取任何属性值，但是性质却不一样，这是开发规范问题，凡是自定义属性都要加上data-前缀哦✅

5. URLSearchParams 假设浏览器的url参数是 "?name=蜘蛛侠&age=16"

```js
new URLSearchParams(location.search).get("name"); // 蜘蛛侠

```

6. hidden 这是一个html属性，规定元素是否隐藏，表现跟css的display: none一致：

```html
<div hidden>我被隐藏了</div>

```
```js
document.querySelector("div").hidden = true / false;

```

7. contenteditable 可以使一个元素可以被用户编辑：

```html
<p contenteditable>我是P元素，但是我也可以被编辑</p>

```

8. spellcheck 也是一个html属性，规定输入的内容是否检查英文的拼写：

```html
<!-- 默认就是true，可省略 -->
<textarea spellcheck="true"></textarea>

```

9. classList 这是一个对象，该对象里封装了许多操作元素类名的方法：

```html
<p class="title"></p>

```
```js
let elem = document.querySelector("p");

// 增加类名
elem.classList.add("title-new"); // "title title-new"

// 删除类名
elem.classList.remove("title"); // "title-new"

// 切换类名（有则删、无则增，常用于一些切换操作，如显示/隐藏）
elem.classList.toggle("title"); // "title-new title"

// 替换类名
elem.classList.replace("title", "title-old"); // "title-new title-old"

// 是否包含指定类名
elem.classList.contains("title"); // false

```

10. getBoundingClientRect 可以获取指定元素在当前页面的空间信息：
```js
elem.getBoundingClientRect();

// 返回
/*
{
  x: 604.875,
  y: 1312,
  width: 701.625,
  height: 31,
  top: 1312,
  right: 1306.5,
  bottom: 1343,
  left: 604.875
}
*/
```

11. contains 可以判断指定元素是否包含了指定的子元素：

```html
<div>
  <p></p>
</div>

```
```js
document.querySelector("div").contains(document.querySelector("p")); // true

```

12. online state 监听当前的网络状态变动，然后执行对应的方法：

```js
window.addEventListener("online", xxx);

window.addEventListener("offline", () => {
  alert("你断网啦！");
});

```

13. battery state 获取设备的电池状态：

```js
navigator.getBattery().then(battery => console.log(battery));

// 返回
/*
{
  charging, // 是否在充电
  chargingTime, // 充满电所需时间
  dischargingTime, // 当前电量可使用时间
  level, 剩余电量

  onchargingchange, // 监听充电状态变化
  onchargingtimechange, // 监听充满电所需时间变化
  ondischargingtimechange, // 监听当前电量可使用时间变化
  onlevelchange // 监听电量变化
}
*/
```
使用场景：提示用户电量已充满，或者为了让用户有安全感，电量低于99%的时候来个弹框提示"该充电啦"✅

14. vibration 嘻嘻，使设备进行震动：

```js
// 震动一次
navigator.vibrate(100);

// 连续震动，震动200ms、暂停100ms、震动300ms
navigator.vibrate([200, 100, 300]);

```
效果如下：不好意思你得用你自己的手握住手机才能感受得到;使用场景：通过振动来提供感官反馈，比如太久没有触摸屏幕的时候连续震动提醒用户✅

15. page visibility 顾名思义，这个API是用来监听页面可见性变化的，在PC端标签栏切换、最小化会触发、在移动端程序切到后台会触发，简单说就是页面消失了🤦‍♂️

```js
document.addEventListener("visibilitychange", () => {
  console.log(`页面可见性：${document.visibilityState}`);
});

```
使用场景：当程序切到后台的时候，如果当前有视频播放或者一些动画执行，可以先暂停✅

16. deviceOrientation 陀螺仪，也就是设备的方向，又名重力感应，该API在IOS设备上失效的解决办法，将域名协议改成https；从左到右分别为alpha、beta、gamma;

```js
window.addEventListener("deviceorientation", event => {
  let {
    alpha,
    beta,
    gamma
  } = event;

  console.log(`alpha：${alpha}`);
  console.log(`beta：${beta}`);
  console.log(`gamma：${gamma}`);
});

```
使用场景：页面上的某些元素需要根据手机摆动进行移动，达到视差的效果，比如王者荣耀进入游戏的那个界面，手机转动背景图会跟着动😂

17. toDataURL 这个canvas的API，作用是将画布的内容转换成一个base64的图片地址；

```js
let canvas = document.querySelector("canvas");
let context = canvas.getContext("2d");

// 画东西
...

let url = canvas.toDataURL("image/png"); // 将画布内容转换成base64地址

```
使用a标签进行图片下载时，图片链接跨域（图片是我的掘金头像），无法进行下载而是进行图片预览：

```html
<img src="xxx">

<button>
  <a href="xxx" download="avatar">下载图片</a>
</button>

```
封装以下代码便可解决✅
```js
const downloadImage = (url, name) => {
  // 实例化画布
  let canvas = document.createElement("canvas");
  let context = canvas.getContext("2d");

  // 实例化一个图片对象
  let image = new Image();
  image.crossOrigin = "Anonymous";
  image.src = url;

  // 当图片加载完毕
  image.onload = () => {
    // 将图片画在画布上
    canvas.height = image.height;
    canvas.width = image.width;
    context.drawImage(image, 0, 0);

    // 将画布的内容转换成base64地址
    let dataURL = canvas.toDataURL("image/png");

    // 创建a标签模拟点击进行下载
    let a = document.createElement("a");
    a.hidden = true;
    a.href = dataURL;
    a.download = name;

    document.body.appendChild(a);
    a.click();
  }
}

```

18. customEvent 自定义事件，就跟vue里面的on跟emit一样；

监听自定义事件：
```js
window.addEventListener("follow", event => {
  console.log(event.detail); // 输出 {name: "前端宇宙情报局"}
});

```
派发自定义事件：
```js
window.dispatchEvent(new CustomEvent("follow", {
  detail: {
    name: "前端宇宙情报局"
  }
}));

```

19. notification PC端的桌面通知，如网页端的微信，当收到消息时，右下角会出现一个通知（尽管你把浏览器最小化），因为这个通知时独立于浏览器的，是系统的一个原生控件；

```js
const notice = new Notification("前端宇宙情报局", {
  body: "这20个不常用的Web API真的有用吗?，别问，问就是有用🈶",
  icon: "我的掘金头像",
  data: {
    url: "https://www.baidu.com"
  }
});

// 点击回调
notice.onclick = () => {
  window.open(notice.data.url); // 当用户点击通知时，在浏览器打开百度网站
}

```
注意：想要成功的调起通知，首先要用户的授权✅
```js
Notification.requestPermission(prem => {
  prem == "granted" // 同意
  prem == "denied" // 拒绝
})

```
所以，再调用之前先向用户发起请求：
```js
let permission = Notification.permission;

if (permission == "granted") {
  // 已同意，开始发送通知
  ...
} else if (permission == "denied") {
  // 不同意，发不了咯
} else {
  // 其他状态，可以重新发送授权提示
  Notification.requestPermission();
}

```

20. fullScreen 全屏不咯? 之前的一个项目刚好用上，不仅仅可以作用在documentElement上，还可以作用在指定元素；

```js
/**
 * @method launchFullScreen 开启全屏
 * @param {Object} elem = document.documentElement 作用的元素
 */
const launchFullScreen = (elem = document.documentElement) => {
  if(elem.requestFullScreen) {
    elem.requestFullScreen();
  } else if(elem.mozRequestFullScreen) {
    elem.mozRequestFullScreen();
  } else if(elem.webkitRequestFullScreen) {
    elem.webkitRequestFullScreen();
  }
}

```
作用在documentElement上没啥可以介绍的咯，就相当于F11开启全屏，那么作用在指定元素会是什么效果呢？就像效果图一样，会直接开启全屏，并且只显示指定的元素，元素的宽高填充了整个屏幕✅关闭全屏的时候需要注意的是，统一用document对象：

```js
/**
 * @method exitFullScreen 关闭全屏
 */
const exitFullScreen = () => {
  if (document.exitFullscreen) {
    document.exitFullscreen();
  } else if (document.mozCancelFullScreen) {
    document.mozCancelFullScreen();
  } else if (document.webkitCancelFullScreen) {
    document.webkitCancelFullScreen();
  }
}

```
使用场景：需要让用户专注去做某件事，比如代码编辑区的全屏✅

21. orientation 可以监听用户手机设备的旋转方向变化；
```js
 window.addEventListener("orientationchange", () => {
  document.body.innerHTML += `<p>屏幕旋转后的角度值：${window.orientation}</p>`;
}, false);

```
也可以使用css的媒体查询：
```css
/* 竖屏时样式 */
@media all and (orientation: portrait) {
  body::after {
    content: "竖屏"
  }
}

/* 横屏时样式 */
@media all and (orientation: landscape) {
  body::after {
    content: "横屏"
  }
}

```
使用场景：页面需要用户开启横屏来获得更好的体验，如王者荣耀里面的活动页😂


























