---
title: vue集成Electron
tags: vue
notebook: wiki
---
- ### 基础环境准备

##### 1\. NodeJs：[http://nodejs.cn/download/](https://links.jianshu.com/go?to=http%3A%2F%2Fnodejs.cn%2Fdownload%2F)

```bash
C:\Users\Administrator>node -v
v12.18.2

C:\Users\Administrator>npm -v
6.14.5
```

##### 2\. Vue-Cli (全局安装Vue-Cli)

> npm install -g @vue/cli

```bash
C:\Users\Administrator>vue -V
@vue/cli 4.5.4
```

___

- ### 搭建Vue项目

### 1\. 可直接通过 WebStorm 创建，也可直接通过命令行创建 (demo为项目名称)

> vue create demo

### 2\. 我这边创建项目时选择的是最简化配置

- ### 选择手动来进行配置

```bash
Vue CLI v4.5.4
? Please pick a preset: (Use arrow keys)
  Default ([Vue 2] babel, eslint)
  Default (Vue 3 Preview) ([Vue 3] babel, eslint)
 > Manually select features 
```

- ### 我这边只选择一个Router路由，其他要用的话后面再加都行

```bash
Vue CLI v4.5.4
? Please pick a preset: Manually select features
? Check the features needed for your project:
 (*) Choose Vue version
 (*) Babel
 ( ) TypeScript
 ( ) Progressive Web App (PWA) Support
>(*) Router
 ( ) Vuex
 ( ) CSS Pre-processors
 ( ) Linter / Formatter
 ( ) Unit Testing
 ( ) E2E Testing
```

- ### 选择的是Vue2.x ，3.x配置项很多都变了搞不懂

```
Vue CLI v4.5.4
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router
? Choose a version of Vue.js that you want to start the project with
> 2.x
  3.x (Preview)
```

- ### 问路由是否使用`history`模式？这里选 n ，因为使用`history`后面打包后无法访问资源

```
Vue CLI v4.5.4
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) (Y/n) n 
```

- ### 选择 Babel、PostCSS、ESLint等配置文件存放位置，这里选择 In dedicated config files (单独保存在各自的配置文件中)

```bash
Vue CLI v4.5.4
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Where do you prefer placing config for Babel, ESLint, etc.? (Use arrow keys)
> In dedicated config files
  In package.json
```

- ### 是否记录一下以便下次使用这套配置，这里选择 N 不记录

```bash
Vue CLI v4.5.4
? Please pick a preset: Manually select features
? Check the features needed for your project: Choose Vue version, Babel, Router
? Choose a version of Vue.js that you want to start the project with 2.x
? Use history mode for router? (Requires proper server setup for index fallback in production) No
? Where do you prefer placing config for Babel, ESLint, etc.? In dedicated config files
? Save this as a preset for future projects? (y/N) N 
```

- ### 确定后，等待Vue-cli完成初始化

```
Vue CLI v4.5.4
✨  Creating project in C:\Users\Administrator\Desktop\test\demo.
�  Initializing git repository...
⚙️  Installing CLI plugins. This might take a while...


> core-js@3.6.5 postinstall C:\Users\Administrator\Desktop\test\demo\node_modules\core-js
> node -e "try{require('./postinstall')}catch(e){}"


> ejs@2.7.4 postinstall C:\Users\Administrator\Desktop\test\demo\node_modules\ejs
> node ./postinstall.js

added 1203 packages from 900 contributors and audited 1206 packages in 130.865s

46 packages are looking for funding
  run `npm fund` for details

found 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
�  Invoking generators...
�  Installing additional dependencies...

added 4 packages from 1 contributor and audited 1210 packages in 16.169s

46 packages are looking for funding
  run `npm fund` for details

found 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
⚓  Running completion hooks...

�  Generating README.md...

�  Successfully created project demo.
�  Get started with the following commands:

 $ cd demo
 $ npm run serve
```

- ### 进入项目运行是否正常

```
C:\Users\Administrator\Desktop\test\demo>npm run serve

> demo@0.1.0 serve C:\Users\Administrator\Desktop\test\demo
> vue-cli-service serve

 INFO  Starting development server...
98% after emitting CopyPlugin

 DONE  Compiled successfully in 1553ms                                            11:47:13 ├F10: AM┤


  App running at:
  - Local:   http://localhost:8080/
  - Network: unavailable

  Note that the development build is not optimized.
  To create a production build, run npm run build.

```

![](https://upload-images.jianshu.io/upload_images/3721308-87d272f2c0410890.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

___

- ### 安装Electron

### 1\. 安装 [electron](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.electronjs.org%2Fdocs)

> npm install --save-dev electron

```
C:\Users\Administrator\Desktop\test\demo>npm install --save-dev electron

> electron@9.2.1 postinstall C:\Users\Administrator\Desktop\test\demo\node_modules\electron
> node install.js

Downloading electron-v9.2.1-win32-x64.zip: [=============================] 100% ETA: 0.0 seconds
+ electron@9.2.1
added 3 packages from 8 contributors, updated 1 package and audited 1270 packages in 5888.384s

46 packages are looking for funding
  run `npm fund` for details

found 1 high severity vulnerability
  run `npm audit fix` to fix them, or `npm audit` for details
```

### 2\. 安装 [electron-builder](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.electronjs.org%2Fdocs%2Ftutorial%2Fboilerplates-and-clis%23electron-builder) (打包使用)

> yarn add electron-builder --dev

```
C:\Users\Administrator\Desktop\test\demo>yarn add electron-builder --dev
yarn add v1.22.4
info No lockfile found.
warning package-lock.json found. Your project contains lock files generated by tools other than Yarn. It is advised not to mix package managers in order to avoid resolution inconsistencies caused by unsynchronized lock files. To clear this warning, remove package-lock.json.
[1/4] Resolving packages...
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi@15.1.1: joi is leaving the @hapi organization and moving back to 'joi' (https://github.com/sideway/joi/issues/2411)
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > request@2.88.2: request has been deprecated, see https://github.com/request/request/issues/3142
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi > @hapi/bourne@1.3.2: This version has been deprecated and is no longer supported or maintained
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi > @hapi/address@2.1.4: This version has been deprecated and is no longer supported or maintained
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi > @hapi/topo@3.1.6: This version has been deprecated and is no longer supported or maintained
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi > @hapi/hoek@8.5.1: This version has been deprecated and is no longer supported or maintained
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > @hapi/joi > @hapi/topo > @hapi/hoek@8.5.1: This version has been deprecated and is no longer supported or maintained
warning @vue/cli-plugin-babel > @vue/cli-shared-utils > request > har-validator@5.1.5: this library is no longer supported
warning @vue/cli-plugin-babel > webpack > watchpack > watchpack-chokidar2 > chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
warning @vue/cli-plugin-babel > webpack > watchpack > watchpack-chokidar2 > chokidar > fsevents@1.2.13: fsevents 1 will break on node v14+ and could be using insecure binaries. Upgrade to fsevents 2.
warning @vue/cli-plugin-babel > webpack > micromatch > snapdragon > source-map-resolve > resolve-url@0.2.1: https://github.com/lydell/resolve-url#deprecated
warning @vue/cli-plugin-babel > webpack > micromatch > snapdragon > source-map-resolve > urix@0.1.0: Please see https://github.com/lydell/urix#deprecated
warning @vue/cli-service > webpack-dev-server > chokidar@2.1.8: Chokidar 2 will break on node v14+. Upgrade to chokidar 3 with 15x less dependencies.
[2/4] Fetching packages...
info fsevents@2.1.3: The platform "win32" is incompatible with this module.
info "fsevents@2.1.3" is an optional dependency and failed compatibility check. Excluding it from installation.
info fsevents@1.2.13: The platform "win32" is incompatible with this module.
info "fsevents@1.2.13" is an optional dependency and failed compatibility check. Excluding it from installation.
[3/4] Linking dependencies...
[4/4] Building fresh packages...
success Saved lockfile.
success Saved 764 new dependencies.
info Direct dependencies
├─ @vue/cli-plugin-babel@4.5.4
├─ @vue/cli-plugin-router@4.5.4
├─
.......
```

### 3\. 新建 Electron 入口，配置到 `package.json` 中

- ### 我这边建立 electron 目录来单独存放 electron 相关文件

- ### package.json 文件中添加 `main` 入口，指向 electron 入口 js 文件，其他都是默认配置

![](https://upload-images.jianshu.io/upload_images/3721308-fa4bdb25632e0e1f.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

- ### electron 中的 `main.js` 入口文件为文档中一致，暂时未做更改，后面再来调整

```
const { app, BrowserWindow } = require('electron')

function createWindow () {
    
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: true
        }
    })

    
    win.loadFile('index.html');

    
    win.webContents.openDevTools()
}



app.whenReady().then(createWindow)


app.on('window-all-closed', () => {
    
    
    if (process.platform !== 'darwin') {
        app.quit()
    }
})

app.on('activate', () => {
    
    
    if (BrowserWindow.getAllWindows().length === 0) {
        createWindow()
    }
})



```

___

### ★ 所有准备工资都已经完成了，但是目前Vue项目和Electron是独立的，接下来看如何在Vue中使用Electron模块等

- ### Vue 集成 Electron

#### ✎ 开发环境

##### ▶ 先来搭建开发环境，启用Vue服务的时候将在Electron中运行

### ? Electron 中需要加载本地页面，但是Vue项目没有发布时，是没有页面提供加载的，后面看到了 Electron 文档中 提供 `loadURL` 可直接加载URL连接，那么就有了下面的思路

### ✔ 思路：因为Vue运行后会提供连接供浏览器访问，而Electron可直接加载URL进行访问，那么就按照这样开搞

### 1\. 修改 electron 的 `main.js` 文件，实现直接加载 Vue 服务地址

```
function createWindow () {
    
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: true
        }
    })

    
    win.loadURL('http://localhost:8080/');

    
    win.webContents.openDevTools()
}
```

### 2\. 既然加载的是服务地址，那就先得把Vue服务启动起来，然后在运行Electron才行

### ① ✘ 方式一 (失败)：在 `package.json` 中添加一条脚本，先启动Vue服务，然后运行Electron，在scripts中添加了一条 `electron-dev` 脚本

```
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "electron-dev": "vue-cli-service serve && electron ."
  },
```

### 失败原因：执行了 `vue-cli-service serve` 后，因为脚本未终止，根本执行不到后面的 `electron .`，所以导致没办法启动 Electron

### ② ✔ 方式二 (可行)：既然①这种方式行不通，但是流程应该是正确的，所以我们换个方式来分别执行这两条命令，等Vue启动后我们在执行 `electron .`

### 通过继续查阅资料得知 nodejs 可直接执行命令行，所以我们继续

- ### 新建了一个配置文件，用于使用 NodeJs 来执行`vue-cli-service serve` 和 `electron .`

![](https://upload-images.jianshu.io/upload_images/3721308-fe00cd29cc974bf0.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

### 配置文件如下

```
const {exec} = require('child_process');



let dev_vue_process = null;

let dev_electron_process = null;

init()

function init() {
    
    if(dev_vue_process == null){
        
        dev_vue_process = exec('vue-cli-service serve',{});

        dev_vue_process.stdout.on('data', data => {
            electronLog(data)
            
            if(data.indexOf("http") > -1){
                
                startElectron()
            }
        })
        dev_vue_process.stderr.on('data', data => {
            if(data.indexOf("<s> [webpack.Progress]") < 0){
                electronLog(data)
            }
        })
    }
}


function startElectron() {
    if(dev_electron_process == null){
        
        dev_electron_process = exec('electron .',{});
        dev_electron_process.stdout.on('data', data => {
            console.log(data)
        })
        dev_electron_process.stderr.on('data', data => {
            console.log(data)
        })
        dev_electron_process.on('close', () => {
            
            process.exit()
        })
    }
}


function electronLog (data) {
    let log = ''
    data = data.toString().split(/\r?\n/)
    data.forEach(line => {
        log += `  ${line}\n`
    })
    if (/[0-9A-z]+/.test(log)) {
        console.log(
            '┏ Electron -------------------' +
            '\n\n' +
            log +
            '┗ ----------------------------' +
            '\n'
        )
    }
}
```

- ### 修改 `package.js` 文件中的执行命令 `electron-dev` 使用node来执行我们刚才创建的 js 配置文件

```
"scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "electron-dev": "node electron/config/electron-run-dev.js"
  },
```

- ### 我们在试下执行 `electron-dev` 命令，可以看得出已经成功了!✌✌✌

![](https://upload-images.jianshu.io/upload_images/3721308-e3b2fbf8d7f01564.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

##### ▶ 开发环境搭建得差不多了，我们试着在Vue页面中引入Electron进行使用

- ### 准备把 `About.vue` 添加一个按钮点击打开一个新的 electron 窗口

```
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="clickButton">点击查看</button>
  </div>
</template>

<script>
  const electron = require('electron')
  // import electron from 'electron'
  export default {
    methods:{
      clickButton(){
        new electron.remote.BrowserWindow({
          width: 200,
          height: 200
        }).loadURL('http://baidu.com')
      }
    }
  }
</script>

```

- ### 我试了在Vue页面中使用 `require`、`import` 进行引入，都报错

```
const electron = require('electron')
```

```
import electron from 'electron'
```

```
TypeError: fs.existsSync is not a function
    at getElectronPath (index.js?bdb9:7)
    at Object.eval (index.js?bdb9:18)
    at eval (index.js:20)
    at Object../node_modules/electron/index.js (about.js:34)
    at __webpack_require__ (app.js:854)
    at fn (app.js:151)
    at eval (About.vue?c330:9)
    at Module../node_modules/cache-loader/dist/cjs.js?!./node_modules/babel-loader/lib/index.js!./node_modules/cache-loader/dist/cjs.js?!./node_modules/vue-loader/lib/index.js?!./src/views/About.vue?vue&type=script&lang=js& (about.js:11)
    at __webpack_require__ (app.js:854)
    at fn (app.js:151)
```

- ### 没办法，毕竟不是专业前端，查资料吧

- ### 然后了解到可以使用 `window.require` 来代替 `require` 进行导入，试一试，果然可行！这一步的前提是在electron入口js中，创建窗口时需要将 `nodeIntegration` 设置为 true，默认是 false 的

```
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="clickButton">点击查看</button>
  </div>
</template>

<script>
  const electron = window.require('electron')
  export default {
    methods:{
      clickButton(){
        new electron.remote.BrowserWindow({
          width: 200,
          height: 200
        }).loadURL('http://baidu.com')
      }
    }
  }
</script>
```

![](https://upload-images.jianshu.io/upload_images/3721308-af6609c8e2924bde.png)

- ### 好了，目前为止在 Vue 中使用 electron 的问题也解决了！！！当然也可以在 data 属性里面引入 electron

```
<template>
  <div class="about">
    <h1>This is an about page</h1>
    <button @click="clickButton">点击查看</button>
  </div>
</template>

<script>
  export default {
    data(){
      return{
        electron : window.require('electron')
      }
    },
    methods:{
      clickButton(){
        new this.electron.remote.BrowserWindow({
          width: 200,
          height: 200
        }).loadURL('http://baidu.com')
      }
    }
  }
</script>
```

#### ✎ 发布打包

##### ▶ 开发应该差不多了，目前就该考虑打包的问题了

### 先看看原始 Electron 解构目录，这个是 Electron 官方文档上的目录结构

```
your-app/
├── package.json
├── main.js
└── index.html
```

- ### 其中包含了：`package.json`、`main.js`、`index.html`

- ### 通过生成出来的资源文件夹中，也同时存在这三个文件，唯一一点不同的就是`package.json`与我们写的不一样，可能是在build的时候，他自动重新生成的一个

![](https://upload-images.jianshu.io/upload_images/3721308-43003ab3690496a0.png)

- ### 所以打包的时候至少要存在 `main.js`、`index.html`

`main.js`：electron 入口文件  
`index.html`：electron 需要加载的页面

### 得出结论后继续往下走，就看如何来配置了

##### ✔ 思路：`main.js` electron 入口文件我们是有的，但是缺少html加载页面，所以我们还是得必须先把Vue进行build后，然后把`main.js`放到一起去进行打包，当然`main.js`就需要加载Vue进行build后的index.html页面了

### 1.配置Vue生成到指定目录，以及调整访问资源为相对路径，我这边用的 @vue/cli 4 ，所以在项目里面添加配置文件 `vue.config.js`

```
module.exports = {
    
    publicPath: './',
    
    outputDir: 'build/vue-build'
}
```

![](https://upload-images.jianshu.io/upload_images/3721308-9c37e1638d6e7a99.png)

### 这时对Vue进行build的时候，会把编译好的文件放在 build/vue-build 目录下面，因为更改成了相对路径，所以双击就能打开 index.html 页面

### 3.Vue相关就差不多了，接下来把electron打包的配置进行配置下，在`package.json`中配置，增加了 `electron-build` 脚本命令 和 `build` 相关配置

- `electron-builder --dir`：electron-builder 的一个打包命令，不用生成安装包
- `build`：electron-builder 中的一个配置项，具体参数可阅读 electron-builder文档，其中特别说明下  
    `output`：electron 打包到指定的路径  
    `files`：具体打包那些文件夹的内容  
    （因为Vue编译后的目录是 build/vue-build ，所以我选择把Vue编译后的文件夹打包到 electron 中）

```
{
  "name": "demo",
  "version": "0.1.0",
  "private": true,
  "main": "build/vue-build/main.js",
  "scripts": {
    "serve": "vue-cli-service serve",
    "build": "vue-cli-service build",
    "electron-dev": "node electron/config/electron-run-dev.js",
    "electron-build": "electron-builder --dir"
  },
  "build": {
    "productName": "vue-cli-electron",
    "appId": "com.test",
    "asar": false,
    "directories": {
      "output": "build/electron-build"
    },
    "files": [
      "build/vue-build/**/*"
    ]
  },
  "dependencies": {
    "core-js": "^3.6.5",
    "vue": "^2.6.11",
    "vue-router": "^3.2.0"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.5.0",
    "@vue/cli-plugin-router": "~4.5.0",
    "@vue/cli-service": "~4.5.0",
    "electron": "^9.2.1",
    "electron-builder": "^22.8.0",
    "vue-template-compiler": "^2.6.11"
  }
}

```

___

### ➤ 其实按理说配置的差不多了，以目前这种情况来说，也能进行打包操作了，具体如何操作我们往下看

### 1.我们先执行build命令，先将Vue进行编译，会在 build/vue-build 下面生成Vue相关页面

![](https://upload-images.jianshu.io/upload_images/3721308-bd10b19500f0ef79.png)

### 2.根据上面说的情况，我们要把electron的入口文件`main.js`放到 build/vue-build 中，然后执行electron的打包操作

- ### 这里需要将 `mian.js` 中 loadURL 改为加载本地的 index.html ，loadURL 也能加载本地页面，但需要在前面加上 `file://`，这里使用了 path 模块来拼接路径，如果直接使用下面两个方式是不行的

```


win.loadFile("index.html");


win.loadFile("build/vue-build/index.html");
```

```

win.loadURL(path.join("file://", 'index.html'));
```

- ### 所以这里将会使用到 `__dirname` 来做路径拼接

`__dirname`：当前文件的所在绝对路径，如下所示进行拼接

```

win.loadURL(path.join("file://", __dirname, 'index.html'));
```

![](https://upload-images.jianshu.io/upload_images/3721308-3c6fa088ab814b24.png)

### 3.接下来还需要配置下 `package.json` 中的 `main` 入口文件，因为现在要打包，所以得把入口文件也改成 build/vue-build 中的 `main.js`

![](https://upload-images.jianshu.io/upload_images/3721308-0fd47878d1284b8f.png)

### 4.执行 `electron-build` 看下打包情况与运行情况，目前是没有问题的。如果打包慢，下载对应依赖很慢的话，可以参考 [《electron-builder 打包下载依赖慢》](https://www.jianshu.com/p/ef7962a2edf6)这篇文章

![](https://upload-images.jianshu.io/upload_images/3721308-391690d273a29486.png)

___

### ➤ 上面的步骤其实已经可以完成正常的开发了，但是打包过于繁琐，复制改配置啥的，所以我们写个脚本来自动复制，自动改配置

### 1.增加判断为发布还是开发，我们在 electron 入口文件 `main.js` 中增加判断如下

- ### 引入 path 模块

- ### 增加 `process.env.NODE_ENV` 来判断是使用开发时的Url，还是发布后本地的index.html，但是 `process.env.NODE_ENV` 是未定义的，所以我们得在自己的脚本中定义

```
const { app, BrowserWindow } = require('electron')
const path = require('path')


const winURL = process.env.NODE_ENV === 'development'
    ? `http://localhost:9999`
    : path.join("file://", __dirname, 'index.html')

function createWindow () {
    
    const win = new BrowserWindow({
        width: 800,
        height: 600,
        webPreferences: {
            nodeIntegration: true
        }
    })

    
    win.loadURL(winURL);

    
    win.webContents.openDevTools()
}



app.whenReady().then(createWindow)


app.on('window-all-closed', () => {
    
    
    if (process.platform !== 'darwin') {
        app.quit()
    }
})

app.on('activate', () => {
    
    
    if (BrowserWindow.getAllWindows().length === 0) {
        createWindow()
    }
})



```

### 2.增加 `JsonUpUtil.js` 脚本，用于更改 `package.json` 中的入口文件路径

- ### 目前我放置在electron -> config -> util 目录下

```
const fs = require('fs');
const path = require('path')
let packageJson = {}


function changeJson(jsonFile, key, newVal) {
    packageJson = {};
    jsonFile = path.join(__dirname, jsonFile);
    fs.readFile(jsonFile, (err, data) => {
        if (err) {
            console.error(err);
        }
        
        packageJson = data.toString();
        packageJson = JSON.parse(packageJson);

        if(packageJson[key] != newVal){
            
            packageJson[key] = newVal;

            
            let newJson = JSON.stringify(packageJson, null, "\t");
            fs.writeFile(jsonFile, newJson, function (err) {
                if (err) {
                    console.error(err);
                }
            })
        }
    });
}

module.exports = {
    changeJsonMain: (val) => {
        console.log("程序入口：", val)
        changeJson('../../../package.json', 'main', val)
    }
}
```

![](https://upload-images.jianshu.io/upload_images/3721308-2d83eba95bd7ddba.png)

### 3.增加electron打包脚本，实现自动复制 electron 相关文件到 build/vue-build 文件中

- ### 在electron -> config 中增加 `electron-build-pro.js` 脚本

- ### 在脚本中 定义下 `process.env.NODE_ENV` 为生产发布

- ### 使用这个脚本的时候将 `package.json` 中 `main` 入口改为 我们复制到 build/vue-build 中的 main.js

```
const fs = require('fs')
const path = require('path')
const jsonUp = require('./util/JsonUpUtil')


process.env.NODE_ENV = 'production';


jsonUp.changeJsonMain('build/vue-build/main.js');


exists(path.join(__dirname, '../dev'), path.join(__dirname, '../../build/vue-build'), copy );


function copy(src, dst) {
    
    fs.readdir(src, (err, paths)=> {
        if (err) {
            throw err;
        }

        paths.forEach(function (path) {
            let _src = src + '/' + path,
                _dst = dst + '/' + path,
                readable, writable;

            fs.stat(_src, function (err, st) {
                if (err) {
                    throw err;
                }

                
                if (st.isFile()) {
                    
                    readable = fs.createReadStream(_src);
                    
                    writable = fs.createWriteStream(_dst);
                    
                    readable.pipe(writable);
                }
                
                else if (st.isDirectory()) {
                    exists(_src, _dst, copy);
                }
            });
        });
    });
}


function exists(src, dst, callback) {
    fs.exists(dst, function (exists) {
        
        if (exists) {
            callback(src, dst);
        }
        
        else {
            fs.mkdir(dst, function () {
                callback(src, dst);
            });
        }
    });
}
```

![](https://upload-images.jianshu.io/upload_images/3721308-5fe7d0606f0914d1.png)

### 4.`electron-build-pro.js`中我们都定义了`process.env.NODE_ENV`，开发环境脚本`electron-run-dev.js`肯定也需要定义，来进行区分开发环境还是发布环境

- ### 配置 `process.env.NODE_ENV` 为开发环境

- ### 修改 `package.json` 调整为 开发环境

```
const {exec} = require('child_process');
const jsonUp = require('./util/JsonUpUtil')


process.env.NODE_ENV = 'development';


jsonUp.changeJsonMain('electron/dev/main.js');



let dev_vue_process = null;

let dev_electron_process = null;

init()

function init() {
    
    if(dev_vue_process == null){
        
        dev_vue_process = exec('vue-cli-service serve',{});

        dev_vue_process.stdout.on('data', data => {
            electronLog(data)
            
            if(data.indexOf("http") > -1){
                
                startElectron()
            }
        })
        dev_vue_process.stderr.on('data', data => {
            if(data.indexOf("<s> [webpack.Progress]") < 0){
                electronLog(data)
            }
        })
    }
}


function startElectron() {
    if(dev_electron_process == null){
        
        dev_electron_process = exec('electron .',{});
        dev_electron_process.stdout.on('data', data => {
            console.log(data)
        })
        dev_electron_process.stderr.on('data', data => {
            console.log(data)
        })
        dev_electron_process.on('close', () => {
            
            process.exit()
        })
    }
}


function electronLog (data) {
    let log = ''
    data = data.toString().split(/\r?\n/)
    data.forEach(line => {
        log += `  ${line}\n`
    })
    if (/[0-9A-z]+/.test(log)) {
        console.log(
            '┏ Electron -------------------' +
            '\n\n' +
            log +
            '┗ ----------------------------' +
            '\n'
        )
    }
}
```

![](https://upload-images.jianshu.io/upload_images/3721308-8490f602a3d83f15.png)

### 5.在 `package.json` 中修改 `electron-build` 打包命令，使用刚创建的 `electron-build-pro.js` 进行打包操作

- ### 打包步骤：vue编译 -> 执行`electron-build-pro.js`脚本 -> electron 打包

```
"scripts": {
        "vue:serve": "vue-cli-service serve",
        "vue:build": "vue-cli-service build",
        "electron:dev": "node electron/config/electron-run-dev.js",
        "electron:build": "vue-cli-service build && node electron/config/electron-build-pro.js && electron-builder --dir"
    }
```

___

### 好了，到此为止就可以使用 `electron-dev` 进行开发，和使用 `electron-build` 进行打包了

### `electron-build-pro.js` 中，会将 electron -> dev 下面的所有文件夹和文件复制到 build/vue-build 里面一并打包，所以 跟 electron 相关的开发都可以在 electron -> dev 目录下进行开发

相关Demo项目我已上传到 GitHub ，有兴趣的可以看下，毕竟我不是专业前端，突发奇想的来了解了下，有任何建议的小伙伴欢迎留言，互相学习!

Demo项目：[vue-cli-electron](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Fzhuobufan%2Fvue-cli-electron)

更多精彩内容下载简书APP

"小礼物走一走，来简书关注我"

还没有人赞赏，支持一下

[](https://upload.jianshu.io/users/upload_avatars/3721308/afc17a8336cb.jpeg?imageMogr2/auto-orient/strip|imageView2/1/w/100/h/100/format/webp)](https://www.jianshu.com/u/8aa4820e13e0)

总资产11 (约0.80元)共写了1.0W字获得1,230个赞共6个粉丝

### 被以下专题收入，发现更多相似内容
