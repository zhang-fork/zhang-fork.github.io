---
title: "Vue小白实现reactive"
date: 2020-05-23T10:26:19+08:00
draft: false
toc: false
images:
tags: 
  - ["es6","Vue","reactive"]
---

After reading [es5](https://wangdoc.com/javascript/), [es6](https://es6.ruanyifeng.com/), and [ts](https://ts.xcatliu.com/), ok, it is time to read the Vue. So,git clone the source code ,then open it.
~~WTF~~,every single word i understood, but, you know,i gave up.

All paths lead to Rome,as a newbie, what i can do is to write some sample myself.The easiest part of Vue may be the reactive function.So, let's get rid of it.
```JavaScript
//The core concept of reactive is when we change the data,
//the view can update simultaneously.
//Of course,the javaScript do not do this automatically,
//when we change the data of an variable, just change it, nothing happen.
//Luckily,we can solve this by the proxy API in es6.
function reactive(object){
  if(object !== null && typeof object == "object"){
    return makereactive(object)
  }else{
    return object
  }
}
function makereactive(object){
  return new Proxy(object,handler)
}
let handler={
  get(target,key,receiver){
    //we can do what we want do here!
    let res=Reflect.get(target,key,receiver)
    return res
    }
  //ok, you can add other handlers you need ,do it yourself.
}

```
At this stage , problems below should be solved:
* how to make a nested object reactive
* if we use the method of array,the handlers such as getting may be triggered more than once.
* some other problems we don't know, you are the newbie!
***
## Function Reactive
Ok,let's assume that all  problems above had been solved,how about function?


As we know,the procedure ,such as joining or splitting the string,whatever,can also change the date.if we change the string, the returns of function must be changed too.how to implement this?


Simplified answer:if we change the object involved in the function, we recall this function.

Does JS can remember which function should be recalled when we change the object ?

Of course not,we have to  construct a container to remember this,which store the object and the function ,what's more,this object may have many properties,our function may only related one of them,or many other  properties too.

what a mess,whatever, let's call it verybigmap.

```JavaScript
let verybigmap= new WeakMap();
```
* The key is that when we call a function, we want verybigmap to remember and then  we change the object ,call it again the handler getter must be triggered. Obviously,we should implant our code in handler.
```JavaScript
let handler={
  get(target,key,receiver){
    track(target,key)//track is a function that throw target and key into verybigmap
    ...
  }
}

```
And then we make the function reactive
```JavaScript
effect(fn){
  //1,throw the fn to verybigmap
  //2,run fn to trigger the track function
  //which one be first?
}
//I think this name 'effect' get from the terminology `side effect`
```
The problem in above code segments may be not a problem for experts,but no one forbid me to have a question.if we throw the fn to the verybigmap first,where we place it,the *key*,or the *value*?

Leaving it aside first,our aim is,after we trigger the setter(change a data),we recall the function,right? At first ,we must find the function in the verybigmap, at that moment, what we know is the target and the key(from the setter handler). Let's go back to previous problem, the answer is easy, fn must be the value. Yet hard question followed, how can we place a value in a map before we get a key?

It looks like we should run fn first,nevertheless,from the context of effect function,we can not get the key connected to fn.

There must be some kind of bridge to connect the target with fn,regardless of which one run first.you the wise must know that we can use the same trick again,when we run one step, push the key or value in stack,when we run the second procedure,we get the key or value from the stack top,then pop it.wow,there we go,fix it.

This article cost me a little time,I am so tired,the rest will be in next [post]().

To be continued...
