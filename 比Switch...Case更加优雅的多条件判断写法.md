## 前言

前几天，本人负责编写网站上线的环境判断功能，其中涉及到大量多条件判断的code。对于多条件判断写法，大部分程序员都是采用if...else if...else或者switch...case的写法，但是有一种多条件判断写法，比传统写法更加有趣且优雅——对象属性判断法。

## 实例展示

废话不多说了，下面直接用三个实例，给大家分别展示三种不同的多条件判断写法。

1. if...else if...else

```js
function whatFood(mealtime) {
    let food = '';

    if (mealtime === 'breakfasttime') {
        food = 'egg';
    } else if (mealtime === 'lunchtime') {
        food = 'vegetable'
    } else if (mealtime === 'dinnertime') {
        food = 'fruit'
    } else {
        food = 'cookie'
    }

    return food;
}

```

2. switch...case

```js
function whatFood(mealtime) {
    let food = '';

    switch (mealtime) {
        case 'breakfasttime':
            food = 'egg';
            break;
        case 'lunchtime':
            food = 'vegetable';
            break;
        case 'dinnertime':
            food = 'fruit';
            break;
        default:
            food = 'cookie';
    }

    return food;
}

```

3. 对象属性判断法

```js
function whatFood(mealtime) {

    const food = {
        breakfasttime: 'egg',
        lunchtime: 'vegetable',
        dinnertime: 'fruit'
    }

    return food[mealtime] ? food[mealtime] : 'cookie';
}

```

## 总结

相信你通过上面三个实例，已经看到了不同写法孰优孰劣，如果你有更好的多条件判断写法，欢迎留言讨论，祝工作顺利，生活愉快。
