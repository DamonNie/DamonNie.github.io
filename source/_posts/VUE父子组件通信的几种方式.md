---
title: VUE父子组件通信的几种方式
top: false
cover: false
date: 2020-03-12 21:26:55
password:
summary: 
tags:
 - 前端
 - Vue
categories: 前端
---

> VUE 父子组件通信的几种方式

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

## 父组件获取子组件的参数和方法

* ref一般被用来给DOM元素或子组件注册引用信息，子组件也同样适用。这时候创建子组件的时候，同时注册一个ref引用

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



