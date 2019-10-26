---
title: 后台管理系统 vue-admin-webapp
copyright: true
date: 2019-10-20 17:08:52
categories:
 - web前端
tags:
 - Javascript
 - Vue
 - echart
 - fastmock
 - element-ui
 - axios
---
>这个本人第一次做比较大的项目,结合许多知识来完成项目,代码可能不怎么好看,优化也不行,请见谅。在之后我会继续提交的我的实力。因为我还是初学前端不到一年，如果下面的有所错误请提出来我会修改。

# vue-admin-webapp

**看了掘金一篇文章后，仿照别人的demo自己做的一个vue后台管理系统**

>[掘金原文章地址](https://juejin.im/post/5d69f6676fb9a06b0b1c8cd2)

本人Blog：[点击进入](https://yqxshiki.com/)

本项目涉及的技术栈有[vue](https://cn.vuejs.org/)
[vue-cli](https://cli.vuejs.org/zh/guide/) [vue-Router](https://router.vuejs.org/zh/) [axios](http://www.axios-js.com/)  [Echarts](https://www.echartsjs.com/zh/index.html) [element-ui](http://element-ui.cn/#/zh-CN) [fastmock](https://www.fastmock.site/) [webpack](https://www.webpackjs.com/)

本项目github地址[vue-admin-webapp](https://github.com/yqxshiki/vue-admin-webapp)

项目运行地址[vue-admin-webapp](http://yqxshiki.gitee.io/yqx-vue-admin-webapp/#/login)

### 项目简介

vue-admin-webapp 是一个后台管理系统,基于**vuecli** 和**element-ui**,使用fastmock来模拟数据,其中有图表,表格,权限,excel等等，你可以根据你的需求来添加路由。

### 安装

```git
# 克隆项目

git clone git@github.com:yqxshiki/vue-admin-webapp.git

# 进入项目目录
cd vue-admin-webapp

# 安装依赖
npm install

# 启动服务
npm run serve
```

启动后，将自动打开游览器 **http://localhost:8080**,你就可以看到项目效果了。

### 项目页面结构

 ![](https://blog-1259178461.cos.ap-chengdu.myqcloud.com/vue-admin-webapp/page.png)

出去登录页,页面主要来三个部分组成：**头部 侧边栏 展示页**,可以点击侧边栏来就行路由跳转

### 登录权限验证

从fastmock中接收token,登录时存储在**localStorage**,设置全局前置守卫,在进入其他页面时，有token时才能进入，不然就跳到login页面

#### 全局前置守卫

```javascript
router.beforeEach((to, from, next) => {
  const isLogin = localStorage.loginToken ? true : false;
  if (to.path == "/login") {
    next();
  } else {
    isLogin ? next() : next('/login')
  }
})
```

#### 请求拦截

```javascript
axios.interceptors.request.use(config => {
  // 判断是否有token
  if (localStorage.loginToken) {
    config.headers.Authorization = localStorage.loginToken;
  }
  return config;
}, err => {
  // 请求错误
  return Promise.reject(err);
})
```

#### 响应拦截

```javascript
axios.interceptors.response.use(res => {
  return res;
},
  err => {
    const { status } = err.response;
    if (status == 401) {
      // 后台定义401为过期
      alert("token过期,请重新登录!")
      // 清楚token
      localStorage.removeItem("loginToken");
      router.push("/login");
    } else {
      alert(err.response.data)
    }
    return Promise.reject(err);
  });
```

### Echart多图表

会熟练运用Echart,直线图，饼图，柱状图，动态数据图等等,例如下图

![](https://blog-1259178461.cos.ap-chengdu.myqcloud.com/vue-admin-webapp/echart.png)

### Excel

  excel在实际项目中主要是后端做的，当然前端也可以做,只是我觉得现在没有必要所以没做。想了解的可以去搜索一下就有。

### fastmock数据

这里引用官方的介绍
>fastmock可以让你在没有后端程序的情况下能真实地在线模拟ajax请求，你可以用fatmock实现项目初期纯前端的效果演示，也可以用fastmock实现开发中的数据模拟从而实现前后端分离。在使用fastmock之前，你的团队实现数据模拟可能是下面的方案中的一种或者多种
>>* 本地手写数据模拟，在前端代码中产生一大堆的mock代码。
>>* 利用mockjs或者canjs的can-fixture实现ajax拦截，本地配置必要的json规则。
>>* 后端在Controller层造假数据返回给前端。
---------------
**我的fastmock 项目端口**
![](https://blog-1259178461.cos.ap-chengdu.myqcloud.com/vue-admin-webapp/fastmock.png)
