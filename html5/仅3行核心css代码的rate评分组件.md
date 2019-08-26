## 原理

梳理如下：

1.去找个好看的iconfont，Iconfont-阿里巴巴矢量图标库；
2.借用5个radio单选框，把默认样式都去掉，显示默认的星星；
3.用checked伪类监听用户选中✅，由默认的星星变成高亮的星星；
4.然后配合~兄弟操作符把当前选中的所有兄弟元素都一起高亮💥；
5.把5个radio单选框反向排列 ❗；

## 代码

这是我事先生成好的`iconfont`

```html
<link rel="stylesheet" href="//at.alicdn.com/t/font_1356455_c5d3d3ohlbq.css">
```

一个很简洁的布局：

```html
<div class="rate-content">
  <input type="radio" name="rate">
  <input type="radio" name="rate">
  <input type="radio" name="rate">
  <input type="radio" name="rate">
  <input type="radio" name="rate">
</div>
```

先把默认的星星显示出来：

```scss
// 去掉默认样式
input {
  -webkit-appearance: none;
  border: none;
  outline: none;
  cursor: pointer;
}

.rate-content {
  $main: #ffa822; // 高亮颜色
  $basic: #999; // 默认颜色
  
  // 单个星星
  input[name="rate"] {
    font-family: "iconfont"; // 之前引入的iconfont字体
    font-size: 30px;
    padding-right: 10px;
    
    // 默认显示的星星
    &::after {
      content: "\e645";
      color: $basic;
      transition: color .4s ease; // 加点颜色过渡效果
    }
  }
}

```

实现选中单个星星：

```scss
input[name="rate"] {
  // 高亮的星星
  &:checked {
    &::after {
      content: "\e73c";
      color: $main;
    }
  }
}

```

实现连同兄弟元素一起高亮：

```scss
input[name="rate"] {
  // 高亮的星星
  &:checked,
  &:checked ~ input[name="rate"] {
    ...
  }
}

```

然后把input反向排列：

```scss
.rate-content {
  display: flex;
  flex-flow: row-reverse;
}

```

鼠标移入预览选中效果：

```scss
input[name="rate"] {
  // 高亮的星星
  &:checked,
  &:hover {
    &::after {
      content: "\e73c";
      color: $main;
    }
  
    // 兄弟元素一起高亮
    & ~ input[name="rate"] {
      &::after {
        content: "\e73c";
        color: $main;
      }
    }
  }
}

```

加入放大动画

```scss
input[name="rate"] {
  transition: transform .2s ease; // 加入过渡效果
  
  // 高亮的星星
  &:checked,
  &:hover {
    ...
  }
  
  // 鼠标移入使星星放大
  &:hover {
    transform: scale(1.2);
  }
}

```

## 总结

核心代码其实就是这两段，其他都是可选的🈚

元素反向排列：

```text
display: flex;
flex-flow: row-reverse;
```

兄弟元素操作：

```text
input:checked ~ input
```

如果不用flex反向排列，还可以用rotateZ:

```scss
.rate-content {
  display: flex;
  // flex-flow: row-reverse;
  transform: rotateZ(180deg);
}

```

z轴旋转180deg之后发现星星的头跟尾巴反过来了，那么子元素也旋转180deg即可：

```scss
.rate-content {
  input[name="rate"] {
    transform: rotateZ(180deg);
    
    // 鼠标移入使星星放大
    &:hover {
     transform: scale(1.2) rotateZ(180deg);
    }
  }
}

```

## 需要注意的细节

```scss
input[name="rate"] {
  // padding-right: 10px;
  margin-right: 10px;
}

```

如果用外边距的话，那么会出现以下情形:

内边距的作用是保持元素连贯性以及扩大点击范围；


## 关于如何获取分数

原生javascript获取也很简单，不过你得先给五个input增加value或自定义索引值：

```html
<input type="radio" name="rate" value="1">
<input type="radio" name="rate" value="2">
<input type="radio" name="rate" value="3">
<input type="radio" name="rate" value="4">
<input type="radio" name="rate" value="5">
```

假设最大分数为5：

```js
const getScore = () => {
  let checkedInput = document.querySelector("input[name=rate]:checked"); // 获取当前选中的input
  
  // 没选中的时候checkedInput为null，直接返回0
  retrun checkedInput? 5 - Number(checkInput.value) + 1: 0;
}

```

为什么要相减，因为元素是倒序排列的，选中第一个实际上选中的是第五个👌


## 关于如何实现半星星

提供一下思路，十个input，奇数显示星星左边、偶数显示星星右边，其他逻辑不改；

```html
<input type="radio" name="rate" value="1">
<input type="radio" name="rate" value="2">
<input type="radio" name="rate" value="3">
<input type="radio" name="rate" value="4">
<input type="radio" name="rate" value="5">
<input type="radio" name="rate" value="1">
<input type="radio" name="rate" value="2">
<input type="radio" name="rate" value="3">
<input type="radio" name="rate" value="4">
<input type="radio" name="rate" value="5">
```

```scss
 .rate-content input[name="rate"] {
    font-family: "iconfont";
    font-size: 30px;
    /* padding-right: 10px; */
    width: 15px;
    overflow: hidden;
  }

  .rate-content input[name="rate"]:nth-child(odd) {
    direction: rtl;
  }
```

转载：https://juejin.im/post/5d57adf5f265da03e3697e1b
