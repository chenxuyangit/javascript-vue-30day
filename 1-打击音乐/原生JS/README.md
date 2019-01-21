# 01 JavaScript Drum Kit 中文指南

> 作者：
> 简介：[JavaScript30](https://javascript30.com) 是 [Wes Bos](https://github.com/wesbos) 推出的一个 30 天挑战。项目免费提供了 30 个视频教程、30 个挑战的起始文档和 30 个挑战解决方案源代码。目的是帮助人们用纯 JavaScript 来写东西，不借助框架和库，也不使用编译器和引用。现在你看到的是这系列指南的第 1 篇。完整指南在 [GitHub](https://github.com/soyaine/JavaScript30)，喜欢请 Star 哦♪(^∇^*)

## 实现效果

模拟一个打鼓的页面。用户在键盘上按下 ASDFGHJKL 这几个键时，页面上与字母对应的按钮变大变亮，对应的鼓点声音响起来。

## 关键流程

1. 监听键盘事件
2. 播放声音
3. 改变样式

## 步骤拆分

1. **添加键盘监听事件** window.addEventListner('keydwon',fn)
2. **对应事件处理**
    1. **获取键码** event.keyCode
    2. **获取元素** data-key
    3. **处理元素** 播放音乐
3. **改变样式**
    1. **添加监听事件** transitionened事件
    2. **改变样式** classList 添加playing

## 需要知识

### CSS布局
    1. vh,vm  视窗的宽高1vh 等于视窗的1%
        rem  CSS属性 以页面html基准的fontsize字体大小来计算
        https://www.cnblogs.com/lidongfeng/p/7243650.html
    3. min-height 最小高度 可以设置为min-height:100vh 来铺满整个视窗
    4. flex布局
        阮一峰老师的:http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html

### JS语法

ES6
    1. const
    2. 箭头函数 ()=>

`document.querySelectorAll()` 获取一组符合 CSS 选择符的元素快照，类型为 NodeList

NodeList 上有classList 表示改node节点的class所组成的数组 ，可以通过add和remove来添加删除类

```
const audio = document.querySelector(`audio[data-key="${event.keyCode}"]`)
key.classList.add("playing");//添加样式
```
## 解决难点

### 在快速按的时候保证连续响应声音,而不是上次一的声音

在播放之前,设置时间戳为0
```javascript
audio.currentTime =0

```
### 按钮使页面按钮复原?

使用 transitionend 事件可以在 CSS 完成过渡后 删除类
http://www.runoob.com/jsref/event-transitionend.html

```javascript
    keys.forEach(key => key.addEventListener('transitionend', removeClass))

    function removeClass(event) {
            event.target.classList.remove("playing")
        }
```
