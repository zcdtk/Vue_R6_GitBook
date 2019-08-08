## 快速上手


* [本地示例](#本地示例)
    1. [开发环境](#1-开发环境)
    2. [安装依赖](#2-安装依赖)
    3. [远程代理](#3-远程代理)
    4. [启动](#4-启动)
    5. [打包](#5-打包)
    

解决方案通过 `FreeMarker` 引擎构建发布 IBizSys 业务模型成果文件，为开发人员提供良好开发体验。

<blockquote style="border-color: red;">
    <p>
        <strong>
            如果你刚开始学习前端，直接进入开发作为你的第一步可能不是最好的主意 —— 掌握好基础知识再来吧！
        </strong>
    </p>
</blockquote>


### 本地示例

实际项目开发中，解决方案需要根据不同的业务要求，对代码的构建、调试、代理、打包部署等一系列工程化的需求。
项目开发依赖 `Yarn` 和 `Vue Cli (3.0)` 等工具。

#### 1. 开发环境

> 在安装使用 `Yarn` 和 `Vue Cli (3.0)` 前，务必确认 [Node.js](https://nodejs.org) 已经升级到 v4.8.0 或以上，强烈建议升级至最新版本。
> 如果你想了解更多 `Yarn` 工具链的功能和命令，建议访问 [Yarn](https://yarnpkg.com) 了解更多。
> 如果你想了解更多 `Vue Cli (3.0)` 工具链的功能和命令，建议访问 [Vue Cli (3.0)](https://cli.vuejs.org/) 了解更多。

- 访问 [Node.js](https://nodejs.org) ，根据文档安装 `Node.js`。
- 访问 [Yarn](https://yarnpkg.com) ，根据文档安装 `Yarn`。
- 访问 [Vue Cli (3.0)](https://cli.vuejs.org/) ，根据文档安装 `Vue Cli (3.0)`。

<blockquote style="border-color: red;"><p>在安装 Vue Cli (3.0) ,请使用 Yarn 模式全局安装。</p></blockquote>

```bash
$ yarn global add @vue/cli
```

以下为 Windows 环境开发正常配置 
<br>
<br>
![开发环境信息](/imgs/getting-started/development.png)

#### 2. 安装依赖

打开前端项目，进入工作空间下，执行安装依赖命令

```bash
$ yarn install
```

#### 3. 远程代理

修改远程代理文件 [vue.config.js](http://172.16.180.229/wangxiang1/VUE_R6_FTL/blob/master/APP/vue.config.js.ftl) 代理地址

![远程代理地址](/imgs/getting-started/proxy.png)

#### 4. 启动

在工作空间下，执行启动命令

```bash
$ yarn serve
```

启动后，通过 vue.config.js 开发服务 devServer 下配置的本地启动端口号访问开发项目。<br>
示例: 

```bash
$ http://localhost:8111
```

#### 5. 打包

在工作空间下，执行打包命令

```bash
$ yarn build
```

打包完成后，工作空间内有 dist 目录生成，该目录内内容为解决方案的最终交付产物。
