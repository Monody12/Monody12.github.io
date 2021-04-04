title: 微信小程序学习记录
author: John Doe
tags:
  - WeChat
  - miniprogram
categories:
  - miniprogram
date: 2021-02-17 18:26:00
---
### 目录

+ <a href="#微信小程序学习记录-no1">解决小程序云函数调用或者云数据库操作时的异步问题</a>

---
#### <a name="微信小程序学习记录-no1">一、解决小程序 *云函数调用* 或者 *云数据库操作* 时的异步问题 (2021/2/17)</a>

问题的产生：小程序的主界面需要根据用户信息个性化的显示不同的界面内容。由于一开始写代码的时候将获取用户的openid云函数和进行界面信息显示的操作都同在小程序js文件的`Page`下的`onLoad: function (options) `函数中。想着这样可以在页面加载的过程中同时完成获取openid和加载页面这两个操作。但是由于云开发操作（获取openid）的**执行速度较慢**导致**加载页面时向云数据库中请求基于该用户openid的数据为空**，造成的界面加载异常。

解决方法：将加载界面的代码封装为一个函数（```this.initInterface();```），并在请求用户的openid函数成功完成时进行调用。

注意：经测试 仅**promise风格写法**中才能使用云函数或云数据库

```js
getUserOpenid: function () { //获取用户openid

  wx.cloud.callFunction({

   name: 'login'

  }).then(res => {

   app.globalData.userOpenid = res.result.openid,

​    console.log("用户openid", app.globalData.userOpenid);

   this.initInterface(); //初始化用户界面

  }).catch(err => {

   console.log(err);

  })

 },
```