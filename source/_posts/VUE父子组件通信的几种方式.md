---
title: VUE父子组件通信的几种方式
top: false
cover: false
date: 2020-03-12 21:26:55
password:
summary: 记Vue开发过程中遇过的坑
tags:
 - 前端
 - Vue
categories: 前端
---

> VUE 父子组件通信的几种方式

## 父组件获取子组件的参数和方法

> 关键字:$ref、$refs

* ref一般被用来给DOM元素或子组件注册引用信息，子组件也同样适用。

子组件:
````
<template>
    <div>
    message:{{message}}
    </div>
</template>
export default{
    name:'child',
    props:["msg"],
    data(){
        return{
            message:'这是一条子组件信息',
        }
    }
}
````

* 这时候创建子组件的时候，同时注册一个ref引用。这里要注意一点，


````
<child ref="childBody"></child>
export{
    methods:{
        getChildBody(){
            let msg = this.$refs.childBody.message;
        }
    }
}
````

## 父组件给子组件传递数据

* 在组件中动态绑定，静态的prop也可以

父组件:
````
<child :msg="params"></child>
<child msg="这里是一个参数"></child>
````

* 子组件在props中接收父组件传递过来的参数
子组件：
````
<template>
    <div>
    message:{{message}}
    </div>
</template>
export default{
    name:'child',
    props:["msg"],
    data(){
        return{
            message:this.msg,
        }
    }
}
````

### 如何避免父子组件参数传递的同时把对象引用也绑定

* prop的传值是通过对象传递，经常有这么一种情况：双向绑定会造成数据污染，我们可以提供对传入参数的处理解决。

> 关键字:JSON.parse(JSON.stringify())

通过此方法的处理，父组件的参数不再受子组件修改影响

## 子组件接收父组件参数

> 关键字:prop

具体实现参考上一实例

## 子组件调用父组件方法

> 关键字:$emit

* 触发当前实例上的事件。附加参数都会传给监听器回调。

父组件:
````
<child @change="handleChange"></child>
methods:{
    change(val){
        //pending
    }
}
````
* 第二个参数，可以附加参数传回给监听器回调
子组件:
````
this.$emit('change');
this.$emit('change',true)
````
### 如何以便捷的方式双向绑定参数
> 关键字:sync
在vue中，经常有遇到对一个prop双向绑定的情况，但是会带来维护上的情况，因为子组件可以修改父组件。这里，我们推荐`update:myPropName`的模式触发事件取而代之。举一个示例，用以下方法表达对其赋新值的意图：
````
this.$emit('update:title', newTitle)
````
此处父组件可以监听到那个事件，并根据需要更新本地的一个属性。例如：
````
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
````
当然，为方便起见 这里我们为这种模式提供一个简写，即`.sync 修饰符`

## 子组件修改父组件元素，同理也可以调用方法

> 关键字: $parent

* $parent可以调用父实例，如果当前实例有的话。

父组件:
````
<child></child>
methods:{
    change(){
        //pending
    }
}
````
* 第二个参数，可以附加参数传回给监听器回调
子组件:
````
...
methods:{
    childMethod(){
        this.$parent.change();
    }
}
````






