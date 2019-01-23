# 01 JavaScript Drum Kit 中文指南

> 作者：
> 简介：[JavaScript30](https://javascript30.com) 是 [Wes Bos](https://github.com/wesbos) 推出的一个 30 天挑战。项目免费提供了 30 个视频教程、30 个挑战的起始文档和 30 个挑战解决方案源代码。目的是帮助人们用纯 JavaScript 来写东西，不借助框架和库，也不使用编译器和引用。现在你看到的是这系列指南的第 1 篇。完整指南在 [GitHub](https://github.com/soyaine/JavaScript30)，喜欢请 Star 哦♪(^∇^*)

## 实现效果

实现一个模拟菜单的todo-list列表,在页面中添加商品，刷新页面不会消失。额外的功能:清空列表

主要用到了 localStorage

### CSS
- input 的 button外边框 需要设置border:1px solid rgba(0,0,0,0.1) 才可以清除

### 核心流程
1. 使用一个list保存数据,我们后面就维护他来渲染页面.

这里为什么需要JSON.parse? localStorage是Storage的实例对象,他里面只会保存字符串,所以一定要转换为字符串
```javascript
var plates = JSON.parse(localStorage.getItem('items')) || [];
```
2. 监听事件

这里没什么太多重点,就是监听之后触发不同的fn
```javascript
changeItems.addEventListener('submit', addItem)
clearBtn.addEventListener('click', removeItems)
```
3. 事件被触发后,获取数据并且保存在localStorage中。

首先我们需要获取数据并且缓存下来.这里的this指向了form表单,`this.querySelector('[name=item]').value`就是input标签里的值了。
还是得在强调一遍,Storage类型只能存储字符串.所以我们传入数组plates的时候需要转换为字符串
```javascript
 function addItem(e) {
        e.preventDefault();
        // 获取input内的value值
        const itemInput = this.querySelector('[name=item]')
        let text = itemInput.value;
        let text = this.querySelector('[name=item]').value;
        // 转换为一个对象
        const item = {
            text: text
        }
        plates.push(item)
        //JSON转换字符串
        localStorage.setItem('items', JSON.stringify(plates))

        renderLi(platesList, plates);
    }
```
在我们要清空的话不仅要把数组数据清空,还得把浏览器的缓存清空
```javascript
function removeItems() {
        plates = []
        // 如果不加这里 页面刷新还是会出现
        localStorage.setItem('items', JSON.stringify(plates))
        renderLi(platesList, plates)
    }

```
4. 渲染页面

这里有外层有join(''),原因是我们需要将所有的li一次性拼接起来,每次拼接是很消耗性能的
```javascript
function renderLi(platesList, plates) {
        platesList.innerHTML = plates.map((plate, index) => {
            return `
                <li>
                    <span>${plate.text}</span>
                </li>
            `
        }).join('')
    }
```
### 优化

1. 空数据不能提交
2. 每次输出后清空input框
3. 减少DOM操作,缓存DOM
4. 刷新页面首次渲染,如果有localStorage有数据的话就初始化渲染
```
// addItem
if(!text) return

// input框为空
this.querySelector('[name=item]').value = ''

// 缓存DOM
const itemInput = this.querySelector('[name=item]')

//刷新页面后,localStorage有数据的话还需要渲染
if(plates) renderLi(platesList, plates)

```

### 总结

1. [Storage](https://developer.mozilla.org/zh-CN/docs/Web/API/Storage) 对象只能储存字符串！！！ 所以我们set get 数据都必须要转换为字符串
2. localStorage我们可以理解成浏览器定义的变量(var localStroage = 'women输入的东西') 即便我们退出的浏览器,但是电脑里面的缓存依然可以将其读出来

