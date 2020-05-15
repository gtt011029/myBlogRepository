---
title: react 之 refs
abbrlink: 49264
date: 2019-09-22 13:57:18
tags:
---









Refs提供了一种访问在render方法中创建的DOM节点或者React元素的方法，在典型的数据流中，props是父子组件交互的唯一方式，如果父组件想要修改子组件，就通过新的props重新渲染他，如果想要子组件更改父组件，就会给子组件暴露一个方法，供子组件使用来更改父组件。

但凡事都有例外，某些情况下，需要在典型数据流外，强制修改子代，这个时候就可以使用Refs

<!--more-->

咱们可以在组件添加一个ref属性来使用，**该属性的值是一个回调函数**，接受作为其第一个参数的底层DOM元素或组件的挂挂载实例

```jsx
class UnControlledForm extends Component {
  handleSubmit = () => {
    console.log("Input Value: ", this.input.value)
  }
  render () {
    return (
      <form onSubmit={this.handleSubmit}>
        <input
          type='text'
          ref={(input) => this.input = input} />
        <button type='submit'>Submit</button>
      </form>
    )
  }
}
```

注意：input元素有一个ref属性，他的值是一个函数，该函数接收输入的实际DOM元素，然后将其放在实例上，这样就可以在handleSubmit函数内部访问它

经常被误解的只有在类组件中才能使用refs，但是refs也可以通过利用js中的闭包与函数组件一起使用

```jsx
function CustomForm ({handleSubmit}) {
  let inputElement
  return (
    <form onSubmit={() => handleSubmit(inputElement.value)}>
      <input
        type='text'
        ref={(input) => inputElement = input} />
      <button type='submit'>Submit</button>
    </form>
  )
}
```



之前看的ant design pro vue 框架  https://pro.loacg.com/    虽然用的是vue，但在父子组件传值中也是用到这种思想，

```vue
<a-button type="primary" icon="plus" @click="$refs.createModal.add()">新建</a-button>
// 调用create-form中的add（）方法
<create-form ref="createModal" @ok="handleOk" />
// 这边父组件就可以通过createModal使用子组件中的属性和方法
```

