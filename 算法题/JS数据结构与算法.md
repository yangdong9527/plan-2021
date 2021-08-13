# JS数据结构与算法笔记

逐行逐句,认真记录

## 栈

### 栈数据结构

**栈**是一种**后进先出**的有序集合,新添加和待删除的元素都保存再栈的同一端,称之为栈顶,另一端就叫做栈底,在栈里新元素都靠近栈顶,就元素都接近栈底

> 类似生活中的叠放的盘子, 添加和拿走的盘子 总是在上面

### 创建一个基于数组的栈

我们创建一个类来表示一个栈, 然后需要选择一个数据结构来保存栈里的元素,这里我们可以选择一个数组,因为数组允许我们在任何位置来添加和删除元素,因为栈需要满足**后进先出**的原则, 我们可以为我们栈添加一些方法

+ **push()** :  添加元素到栈顶
+ **pop()**: 移除栈顶的元素,同时返回被移除的元素
+ **peek()**: 返回栈顶的元素
+ **isEmpty()**: 如果栈里没有任何元素 返回 true , 否则 返回 false
+ **clear()**: 清空栈里的元素
+ **size()**: 返回栈里的元素个数

```js
class Stack {
	constructor() {
		this.items = []
	}
 	push(element) {
        this.times.push(element)
    }
    pop() {
        return this.items.pop()
    }
    peek() {
        return this.items[this.items.length -1]
    }
    isEmpty() {
        return this.items.length === 0
    }
    clear() {
        this.items = []
    }
    size() {
        return this.items.length
    }
}

// 使用
const stack = new Stack()
```



### 创建一个基于对象的栈

创建一个栈类最简单的方法是使用一个数组来存储其元素,在处理大量元素的时候,我们同样需要评估如何操作数据更加的有效,在使用数组时存在两个缺点

+ 大部分方法的时间复杂度是O(n), 意思就是我们需要迭代整个数组才能找到那个元素
+ 还有一个原因,数组是元素的一个有序集合,为保证有序,所以占用的内参空间更多

所以我们可以用一个对象来存储所有的栈元素,保证顺序和遵循**后进先出**的条件实现

```js
class Stack {
    constructor() {
        this.count = 0
        this.items = {}
    }
    push(element) {
        this.items[this.count] = element
        this.count++
    }
    size() {
        return this.count
    }
    isEmpty() {
        return this.count === 0
    }
    pop() {
        if (this.isEmpty()) return undefined;
        this.count--
        const result = this.items[this.count]
        delete this.times[this.count]
        return result
    }
    peek() {
        if (this.isEmpty()) return undefined;
        return this.items[this.count -1]
    }
    clear() {
        this.items = {}
        this.count = 0
    }
    toString() {
        if (this.isEmpty()) return undefined;
        let objString = `${this.items[o]}`
        for (let i = 1; i < this.count; i++) {
            objString = `${objString},${this.items[i]}`
        }
        return objString;
    }
```

### 保护数据结构内部元素

在创建别的开发这也可以使用的数据结构和对象时,我们希望保护内部元素,只有我们暴露出的方法才能修改内不数据结构

```js
const stack = new Stack()
console.log(Object.getOwnPropertyNames(stack)) // ['items', 'count']
console.log(Object.keys(stack)) // ['items', 'count']
console.log(stack.items)
```

从上面的代码可以看出count和items属性是公开的,这样不好

我们上面使用的是ES6的class创建的类,calss创建类是基于原型的,这种方式不能声明私有属性和方法

#### 使用Symbol实现

ES6新增了一个Symbol的基本类型,它是不可变的,可以用作对象的属性

```js
const _items = Symbol('stackItems')
class Stack {
    constructor() {
        this[_items] = {}
    }
}
```

我们把代码中的所有的items属性都替换成_items就可以了

但是这种方法创建了一个假的私有属性,因为ES6新增了一个`Object.getOwnPropertySymbols()`方法,可以获取到类里面声明的所有的Symblo属性

```js
const stack = new Stack()
const objSymbols = Object.getOwnPropertySymbols(stack)
stack[objSymbols[0]]
```

还是可以操作栈中的数据,不行再换一种

#### 使用WeakMap实现

有一种数据结构可以确保属性的私有,就是`WeakMap`,它可以存储键值对,其中键是对象,值是任意数据类型

```js
const items = new WeakMap()
class Stack {
    constructor() {
        items.set(this, [])
    }
    push(element) {
        const s = item.get(this)
        s.push(element)
    }
}
```

以上代码解释

+ 声明一个WeakMap类型的变量items
+ 在constructor中,以this(Stack类自己的引用)为键,把栈的数组存入items

这样实现了真正的私有变量,采用这种方法,代码的可读性不强,而且在扩展该类时无法继承私有变量

> 在TypeScript中提供给了类访问修饰符,其中的private修饰符表示属性为私有属性, 但是改修是符只有在编译时有用,编译后还是公开的属性

### 用栈解决问题

栈的实际应用非常广泛, 后面还有很多例子, 这里就介绍一个如何解决十进制转二进制问题

**十进制转二进制** : 将十进制数除以2,然后对商取整,直到结果为0

过程大概如下

![十进制转二进制](../assets/Snipaste_2021-08-05_18-17-58.png)

来来来,给公子喂饼

```js
function decimalToBinary(decNumber) {
    const stack = new Stack()
    let number = decNumber
    let rem;
    let result = ''
    while(number > 0) {
        rem = Math.floor(number % 2)
        stack.push(rem)
        number = Math.floor(number / 2)
    }
    while(!stack.isEmpty()) {
        result += stack.pop().toString()
    }
    return result
}
```

稍微修改一下变成十进制转换成 基数为2~36的任意进制

```js
function baseConverter(decNumber, base) {
  const stack = new Stack()
  const disits = '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ'
  let number = decNumber
  let rem
  let result = ''
  if (!(base >=2 && base <= 36)) {
  	return ''
  }
  while(number > 0) {
  	rem = Math.floor(number % base)
  	stack.push(rem)
  	number = Math.floor(number / base)
  }
  while(!stack.isEmpty()) {
  	result += disits[remStack.pop()]
  }
  return result
}
```



### 小结

这一章主要讲了,栈的基础概念,然后分别讲了使用数组和对象来实现栈这种数据类型的各自的优缺点, 补充了如何实现类中私有属性,最后讲了一个栈的应用



## 队列与双端队列

### 队列

首先了解一下什么是队列, 队列是遵顼**先进先出**原则的一组有序的项

然后我们来定义一个都队列,这里我们为了更高效的获取数据,还是使用对象这种数据类型来帮助我们创建类

```js
class Queue {
    constructor() {
        this.count = 0
        this.lowstCount = 0
        this.items = {}
    }
    enqueue(el) {
        this.items[this.count] = el
        this.count++
    }
    dequeue() {
        if (this.isEmpty()) return undefined;
        const result = this.item[this.count]
       	delete this.items[this.lowstCount]
        this.lowstCount++
    }
    isEmpty() {
        return this.count - this.lowstCount === 0
    }
    peek() {
        if(this.isEmpty()) return undefined;
    }
    clear() {
        this.items = {}
        this.count = 0
        this.lowsCount = 0
    }
    toString() {
        if(this.isEmpty()) {
            return ''
        }
        let result = ''
        for(let i = this.lowstCount; i++; i < this.count) {
            result += this.items[i]
        }
        return result
    }
}
```

解释一下,这里的lowsCount表示的是当前队列中的第一项值的索引, 而这里的count还是表示下一项添加进来的索引



### 双端队列

**双端队列**是一种**允许我们同时从前端和后端添加和移除元素的特殊队列**

举一个栗子,类似与生活中排队买票,一个刚买了票的人如果需要再问些问题可以直接去队伍的头部,如果一个人不想等了也可以直接走开

由于是一种特殊的队列,那么就有相同的几个方法: isEmpty clear size 和 toString, 由于允许在两端添加删除元素,于是就有其他的一些特殊的方法,比如:

+ addFront(el): 在前端添加新元素
+ addBack(el): 在后端添加元素
+ removeFront(): 移除前端
+ removeBack(): 移除后端
+ peekFront(): 返回第一个元素
+ peekbakc(): 返回最后一个元素

```js
class Deque {
    constructor() {
        this.items = {}
        this.count = 0
        this.lowstCount = 0
    }
    addFront(el) {
        if(this.isEmpty()) {
            this.addBack(el)
        } else if(this.lowstCount > 0) {
            this.lowstCount--
            this.items[this.lowstCount] = el
        } else {
            for(let i = this.count; i > 0; i--) {
                this.items[i] = this.itmes[i-1]
            }
            this.count++
            this.items[0] = el
        }
    }
}
```

这里主要讲一下这个addFront这个方法,主要会出现三种情况

第一种场景队列为空的情况下,直接使用从后端添加的方法

第二种场景当队列的第一个元素的索引不为0的时候我们直接添加一位

第三种场景当队列的第一个元素的索引为0的时候,我们遍历讲所有元素都向后移动一位,然后将这个元素直接添加到第一位



### 实际应用

#### 循环队列--击鼓传花游戏

```js
// num 表示花传多少次会淘汰一个人
function hotPotato(elementsList, num) {
    const queue = new Queue()
    const elimitatedList = []
    for (i = 0; i < elementsList.length; i++) {
        queue.enqueye(elementslist[i])
    }
    while(queue.size() > 1) {
        for (let i = 0; i < num; i++) {
            queue.enqueue(queue.dequeue())
        }
        elimitatedList.push(queue.dequeue())
    }
    return {
        eliminated: elemitatedList,
        winner: queue.dequeue()
    }
}
```



### 小结

本章介绍了队列这种先进先出的数据结构, 同时扩展了一个双端队列,你可以把它理解为栈和队列的合集



## 链表

链表一种动态的数据结构,我们可以随意的添加或移除项,它会按需进行扩容

### 链表数据结构

**链表**存储有序的元素集合, 链表中的元素在内存中并不是连续放置的, 每个元素中由一个存储元素本身的节点和一个指向下一个元素的引用组成

相比较与传统的数组, 链表的好处在于,添加或移除元素的时候不需要移动其他元素, 需要注意的是在链表中想访问任何元素,都要从起点开始迭代找到直到所需的元素



#### 创建链表

参数说明, count是链表的长度,head是第一个元素,equalsFn使用来比较元素是否相等

要实现链表类 还要实现他的一些方法

+ push(el): 向链表尾部添加一个新元素
+ insert(el, position): 向链表的特定位置插入一个新元素
+ getElementAt(index): 返回链表中特定位置的元素
+ remove(eement): 从链表中移除一个元素
+ indexOf(element): 返回元素在链表中的索引
+ removeAt(position): 从链表的特定位置移除一个元素
+ isEmpty(): 是否为空

```js
import { defaultEquals } from '../util'
export default class LinkedList {
    constructor(equalsFn = defaultEquals) {
        this.count = 0
        this.head = undefined
        this.equalsFn = equalsFn
    }
    push(element) {
        const node = new Node(element)
        let current;
        if (this.head == null) {
            this.head = node
        } else {
            current = this.head
            while (current.next != null) {
                current = current.next
            }
            current.next = node
        }
		this.count++
    }
    removeAt(index) {
        
    }
}
export class Node {
    constructor(element) {
        this.element = element
        this.next = undefined
    }
}
```

