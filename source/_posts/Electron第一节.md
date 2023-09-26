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

在主进程中可以通过process对象访问node的相关变量，需要注意的是在主线程中是没有办法直接操作dom的因为主线程无法访问渲染器，**主线程和渲染线程存在于完全不同的进程**。

```jsx
app.whenReady().then(() => {
    createWindow()
    console.log("测试demo", window, document)
})
```

![Untitled](electron%E7%AC%AC%E4%B8%80%E8%8A%82%200da949a924f14c82a35e59f26099c561/Untitled.png)

但是我们可以通过**预加载**脚本访问这两个渲染器，可以同时访问到window和document以及Node.js环境，具体的方法如下。

```jsx
// preload.js
window.addEventListener('DOMContentLoaded', () => {
  const replaceText = (selector, text) => {
    const element = document.getElementById(selector)
    if (element) element.innerText = text
  }
  for (const dependency of ['chrome', 'node', 'electron']) {
    replaceText(`${dependency}-version`, process.versions[dependency])
  }
})

// 在主线程中使用preload预加载
const createWindow = () => {
    const win = new BrowserWindow({
      width: 800,
      height: 600,
      webPreferences: {
        preload: path.join(__dirname, 'preload.js'),
      }
    })
  
    win.loadFile('index.html')
}
```

### Electron打包

使用Electron Forge进行打包

```jsx
npm install --save-dev @electron-forge/cli
npx electron-forge import
```