# 目标

> **以高效率的方式，编写出高质量的代码，开发出高体验的产品。**

## 高效率

### 技术栈

* **ES5+：** 熟练使用 ES5 及以上的语法，提升开发效率，不要随意引入第三方库。
* **开发框架：**[Vue.js](https://cn.vuejs.org/)
* **PC 端组件库：** [Element](http://element.eleme.io/#/zh-CN)
* **移动端组件库：** [Vant](https://youzan.github.io/vant/#/zh-CN/intro)
* **常用工具库：** [lodash](https://lodash.com/)， [dayjs](https://github.com/iamkun/dayjs)

项目中所用到的东西都包括但并不限于上面库和工具，团队内的成员也都有各自擅长的其它技术，比如 websocket，Electron 等。但上面几样，每个人都需要熟练掌握并使用，不需要把每个API都记住，但至少要知道有哪些东西，能做什么事，遇到对应的问题都有哪些方法解决。

### 开发工具

工欲善其事，必先利其器，一个好的开发环境对于开发效率也有很大的提升。团队使用的编辑器统一使用 [VsCode](https://code.visualstudio.com/)，统一配置。

* 提升效率的常用技巧：[vscode代码片段使用以及相关技巧](document/vsSkill.md)
* 配置：安装[Settings Sync](https://marketplace.visualstudio.com/items?itemName=Shan.code-settings-sync)插件进行配置

### 工程结构

文件命名都使用短横线连接，所有资源都采用就近维护的原则。

```
project
├── public
|   ├── favicon.ico
|   ├── student.html
|   └── manager.html
└── src
    ├── assets // 存放全局公用的静态资源
    |   ├── images
    |   |   ├── boy.png
    |   |   └── girl.png
    |   └── less
    |       └── main.less
    ├── components // 存放全局公用的组件
    |	├── dialog
    |	|   ├── images // 存放该组件需要的图片
    |	|   └── index.vue
    |	└── empty
    |	    └── index.vue
    ├── directive // 存放全局公用的指令
    |	└── v-auth.js
    ├── entries // 存放各端的具体代码
    |	├── student
    |	|   ├── assets // 存放 student 端的公共静态资源
    |	|   |	├── fonts // 存放字体的文件夹
    |	|   |	├── images // 存放图片
    |	|   |	└── less
    |	|   |	    ├── theme.less // 主题样式，主要是覆盖第三方组件库的默认样式
    |	|   |	    └── variable.less // 颜色尺寸等变量
    |	|   ├── components // 存放 student 端的公共组件
    |	|   |	└── content-box
    |	|   |	    ├── images // 存放该组件需要的图片
    |	|   |	    └── index.vue
    |	|   ├── pages // 存放所有页面
    |	|   |	├── home
    |	|   |	|   ├── images // 存放该页面所需的图片
    |	|   |	|   ├── home-list-item.vue
    |	|   |	|   ├── home-list.vue
    |	|   |	|   └── index.vue
    |	|   |	├── user // 一级页面
    |	|   |	└── user-detail // 二级页面
    |	|   ├── router
    |	|   |	└── index.js
    |	|   ├── store
    |	|   |	├── modules
    |	|   |	├── actions.js
    |	|   |	├── index.js
    |	|   |	└── mutations.js
    |	|   ├── app.vue
    |	|   └── main.js
    |	└── manager
    ├── filter // 存放全局公用的指令
    └── utils // 存放全局公用的方法

```

`public` 文件夹下存放各端的 HTML 和 favicon 文件

`src` 文件夹里是所有业务代码

`components` 文件夹里的每个组件都以一个文件夹为单位，以 index.vue 作为入口，即便这个组件只需要一个 index.vue 文件

`pages` 文件夹里放的就是每一个页面，每一个路由就对应一个页面，每一个页面就是一个文件夹，页面的入口都使用 index.vue

### 接口管理与mocker

开发前先约定接口
[接口管理与mocker文档](document/mock.md)

## 高质量

### 代码规范
	html
	css
	js

### code review

### git规范

### 持续集成

### 测试与发布
测试 发布 监控系统

### 数据结构与算法

### 设计模式

### 可维护性原则

## 高体验

多尺寸适配
高性能
交互，首屏加载，加载，空数据状态
