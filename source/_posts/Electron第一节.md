---
title: Electron第一节
date: 2023-09-26 00:22:29
categories: 
- web前端
tags:
- Javascript
- Electron
---
# electron第一节

Date: September 25, 2023
Status: In progress
Text: 1_Bit

### 什么是electron

**Build cross-platform desktop apps with JavaScript, HTML, and CSS ——— electron官网**

简单来说就是，electron就是一个使用js，html和css搭建跨平台桌面应用程序的框架。

每一个electron应用程序的入口都是main文件，这个文件控制了主进程，负责控制应用的整个生命周期，显示原生界面，执行特殊操作并管理渲染进程。

```jsx
// console.log("测试demo")
// main.js
const { app, BrowserWindow } = require('electron')
const createWindow = () => {
    const win = new BrowserWindow({
      width: 800,
      height: 600
    })
  
    win.loadFile('index.html')
}
app.whenReady().then(() => {
    createWindow()
})
// package.json
"scripts": {
  "dev": "electron ."
},
```

PS:需要注意的是,和vue和react框架一样都需要一个index.html文件。

你可能已经注意到了在`main.js` 文件中我们从electron中引入了两个模块`app,BrowserWindow`

- `app` 模块，它控制应用程序的事件生命周期。
- `BrowserWindow` 模块，它创建和管理应用程序窗口。