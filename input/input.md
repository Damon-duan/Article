# input 的哪些事儿

1、如果绝对定位的元素在最下面，键盘弹起时，绝对定位元素会在键盘上面

> 解决办法：使用flex布局实现，有一个flex-shrink可使用，或者监听resize事件，将元素隐藏

```js

export function adapterPosition(selector) {
    if (/iphone/i.test(navigator.userAgent)) return
    const h = window.innerHeight;
    const dom = document.querySelector(selector)
    if (!dom) return
    const display = dom.style.display
    window.addEventListener('resize', () => {
        const height = window.innerHeight
        if (height < h) {
            dom.style.display = 'none'
        } else {
            dom.style.display = display
        }
    });
}

```

2、低版本浏览器，给input设置flex:1之后，将兄弟元素挤出了父元素空间

> 解决办法：具体原因待查，反正需要给input加一个display:block

```html
<div class="item">
    <div class="name">验证码</div>
    <input class="jInput input" type="number" pattern="[0-9]*" placeholder="请输入短信验证码">
    <button class="btn jSendVcodeBtn">
        发送验证码
    </button>
</div>

```
```css
.item {
    margin-left: 15px;
    box-sizing: border-box;
    height: 60px;
    padding: 12px 15px 12px 0;
    overflow: hidden;
    background-color: #fff;
    color: #212121;
    position: relative;
    display: -webkit-box;
    display: flex;
    -webkit-box-align: center;
    align-items: center;
    border-bottom: 1px solid #f4f4f4;
    font-size: 16px;
}

.item .name {
    margin-right: 10px;
    min-width: 48px;
}

.item .input {
    display: block; // 需要加一个display：block
    outline: 0 none;
    -webkit-box-flex: 1;
    flex: 1;
    font-size: 16px;
    padding: 0;
    border-width: 0;
    box-shadow: none;
    -webkit-user-select: text;
    -moz-user-select: text;
    -ms-user-select: text;
    user-select: text;
}
```

3、relative top失效

> 解决办法：关于relative元素top属性失效的原因，父元素没有设置高度，子元素top使用百分比的时候没有参照，此时可以使用px值

4、滚动穿透问题

> 描述：有场景需要mask，但是背景还是可以滚动，即滚动穿透，解决方案如下，主要是获取页面的滚动元素并设置其为fixed

```css
body.no-scroll {
  position: fixed;
  width: 100%;
}
```

```js
UtilFn.ModalHelper = function (bodyCls) {
    var scrollTop;
    var scrollingElement = document.scrollingElement || document.body; // 此写法兼容webkit，获取页面滚动元素
    return {
        afterOpen: function () {
            scrollTop = scrollingElement.scrollTop;
            document.body.classList.add(bodyCls);
            document.body.style.top = -scrollTop + 'px';
        },
        beforeClose: function () {
            document.body.classList.remove(bodyCls);
            scrollingElement.scrollTop = scrollTop;
        }
    };
}
```

5、ios和安卓配置数据键盘

> pattern用于验证表单输入的内容，通常HTML5的type属性，比如email、tel、number、data类、url等，已经自带了简单的数据格式验证功能了，加上pattern后，前端部分的验证更加简单高效了。显而易见，pattern的属性值要用正则表达式。

常用的正则表达式

pattern的用法都一样，这里不再啰嗦各种详细写法了，只是列出来一些常用的正则就好了：

* 信用卡  [0-9]{13,16}
* 银联卡  ^62[0-5]\d{13,16}$
* Visa: ^4[0-9]{12}(?:[0-9]{3})?$
* 万事达：^5[1-5][0-9]{14}$
* QQ号码： [1-9][0-9]{4,14}
* 手机号码：^(13[0-9]|14[5|7]|15[0|1|2|3|5|6|7|8|9]|18[0|1|2|3|5|6|7|8|9])\d{8}$
* 身份证：^([0-9]){7,18}(x|X)?$
* 密码：^[a-zA-Z]\w{5,17}$ 字母开头，长度在6~18之间，只能包含字母、数字和下划线
* 强密码：^(?=.*\d)(?=.*[a-z])(?=.*[A-Z]).{8,10}$ 包含大小写字母和数字的组合，不能使用特殊字符，长度在8-10之间
* 7个汉字或14个字符：^[\u4e00-\u9fa5]{1,7}$|^[\dA-Za-z_]{1,14}$
