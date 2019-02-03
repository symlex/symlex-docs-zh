# 用户界面

我们位于 `frontend` 目录中的样板代码是为了您的方便，让您直接建立令人印象深刻的单页应用程序 [ Webpack ](https://webpack.js.org/)，[ Vue.js ](https://vuejs.org/) 和 [ Vuetify ](https://vuetifyjs.com/en/)。

![Screenshot](../img/screenshot.jpg)

## 框架 ##

[Vuetify ](https://vuetifyjs.com/en/getting-started/quick-start) 是一个强大的前端 [ Material Design ](https://material.io/) UI 组建框架，用于建立现代化单页应用。

他基于 [ VueJS ](https://vuejs.org/v2/guide/)， 一个 JavaScript 库，结合了最好的想法源自 [ AngularJS ](https://angularjs.org/) (Google) and [React](https://reactjs.org/) (Facebook); 开发是社区驱动的， API 相当稳定。

Vuetify 和 VueJS 初始化在[ frontend/src/app.js ](https://github.com/symlex/symlex/blob/master/frontend/src/app.js)。 [ Webpack ](https://webpack.js.org/concepts/) 用作模块加载器/捆绑器。 它从原始源代码在服务器资产公共构建目录中创建单个优化的JS和CSS文件。 您可以在中找到构建配置 `frontend/webpack.config.js`.

## 构建 ##

可以通过运行`npm run dev`（监视更改并在需要时重新构建）或`npm run build`（单个构建）来触发构建，可以在 **前端目录** 中找到。

[NPM](https://www.npmjs.com/) 是 [NodeJS](https://nodejs.org/en/docs/guides/)的默认包管理器， 一个JavaScript运行时环境，用于在浏览器之外执行JavaScript代码。 我们只将它用作此项目中的任务运行器。 应使用安装和更新依赖关系 [Yarn](https://yarnpkg.com/en/docs/getting-started)。 它与 NPM 兼容，但速度更快，更适合前端开发。

## 依赖 ##

完整的依赖列表可以在 `frontend/package.json` 中找到。 您需要在 **前端目录** 中运行 `yarn install` 来安装它们（在安装过程中自动发生，请参阅 `build.xml` ）。 运行 `yarn add [package name]` 添加新包（库或框架）。
