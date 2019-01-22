# 01 JavaScript Drum Kit 中文指南

> 作者：
> 简介：[JavaScript30](https://javascript30.com) 是 [Wes Bos](https://github.com/wesbos) 推出的一个 30 天挑战。项目免费提供了 30 个视频教程、30 个挑战的起始文档和 30 个挑战解决方案源代码。目的是帮助人们用纯 JavaScript 来写东西，不借助框架和库，也不使用编译器和引用。现在你看到的是这系列指南的第 1 篇。完整指南在 [GitHub](https://github.com/soyaine/JavaScript30)，喜欢请 Star 哦♪(^∇^*)

## 实现效果

实现一个时钟，会根本地时间同步旋转

## 关键流程

1. 实现时钟指针旋转效果
2. 时间变化同步更新指针

## 需要知识

### CSS
- `transfrom-origin`
- `transfrom: rotate()`
- `transition`
- `marigin: 负值`

### 流程
1. 设置指针圆心，旋转的中心,origin设置类似position定位,位置在距离div指针left:0,top:50%的位置

```css
transform-origin: 0 50%;
```

2. 设置指针的过渡时间

```css
transition: all 1s;
```
3. 指针回弹效果

```css
transition-timing-function: cubic-bezier(0.1, 1.5, 0.1, 1);
```
### 解决难点
1. 让整个div盒子居中，我们用了margin:-px
```
margin-top: -5px
```
2. 在指针旋转至0点的最后一刻,使用date.getSeconds之后超过60秒后，又变为0了，这样指针会逆时针旋转1全回到原定，这不符合我们开始的想法

- 解决方案1. 在最后一刻去除掉transition的过渡效果,但是去除了过渡动画,最后一下会十分的僵硬
```javascript
if (nowHour === -90) hourHand.style.transition = 'all 0s'
        // else hourHand.style.transition = 'all 0.5s'
```

- 解决方案2. 初始化之后,我们将初始化的日期缓存起来,更新数据只通过在首次
```javascript
let hour = 0, min = 0, second = 0;
function upDate(time) {
        hour += (1 / 60 / 60 / 12) * 360 * time / 1000
        min += (1 / 60 / 60) * 360 * time / 1000
        second += (1 / 60) * 360 * time / 1000
        console.log(hour, min, second)
        hourHand.style.transform = `rotate(${hour}deg)`
        minHand.style.transform = `rotate(${min}deg)`
        secondHand.style.transform = `rotate(${second}deg)`
    }

```

### 扩展,小总结
1. 想自定义刷设置每次刷新的间隔时间！

既然如此，那么我们需要在更新的时候传递参数进去，可是setInterval里面你写fn(props),这样传递的是fn调用的结果了，是不能这样写的.
然后我们这样通过函数柯里化的方式传递了参数.

```javascript
// 中间函数 用来给upDate传参
    let UPDATE_TIME = 1000;
    //...
    function fn(){
        return upDate(UPDATE_TIME)
    }
    function upDate(time){
        //...
    }
    //...
    setInterval(fn, UPDATE_TIME)

```

**虽然最后我这个想法没啥卵用**,因为开始的时候css的transition过渡动画就已经是1s了,更新时间设置长了2秒钟更新一次过渡动画也只有1s。

最后过渡动画我还是设置成了transition: linear 1s;还是一直转悠比较好看

