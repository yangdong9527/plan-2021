### setup相关

#### Suspense组件

当想在`setup`使用 `async/await`时, 就需要在父组件中将当前组件使用`Suspense`组件包裹起来,不然`setup`返回一个`Promise`会报错



#### setup参数

```js
setup(props, { attrs, slots, emit })
```



### ref 获取元素

模板语法没有发生改变

```html
<div ref="test">test</div>
```

在`setup`中获取元素

```js
setup() {
  const test = ref<HTMLElement|null>(null)
  return {
    test
  }
}
```



### toRefs/toRef

#### toRefs

`toRefs`可以将`reactive`的每一项都转化为`Ref`对象, 一般用来结构`reactive`

```js
//in setup
const user = reactive({name: 'yd'})
const { name } = toRefs(user)
```



#### toRef

相比较于`toRefs`,`toRef`的区别就是只能单独转换一个

```js
useSomeFeature(toRef(props, 'name'))
```

拷贝了一份新的数据值单独进行操作, 更新时相互不影响



