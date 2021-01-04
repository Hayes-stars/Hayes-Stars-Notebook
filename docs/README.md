# Hayes-Stars-Notebook

## 1、安装 node 和 npm

```
这就不详细说了，网上一搜一大堆。这边给个链接。

https://www.cnblogs.com/xilifeng/p/5538711.html

```

## 2、全局安装 docsify

```
npm i docsify-cli -g

```

## 3、使用 docsify 创建文档网站

- 1、在 github 中新建一个项目

  这个项目用来存放我们的文档内容，后面通过 github 来发布我们的文档网站。关于 github 上如何创建项目，如何 clone 到本地，这里就不详细说了。

  将项目 clone 到本地:

  git clone **\*\***

- 2、初始化项目
  - 进入 clone 的项目中执行：
  ```
    docsify init ./docs
  ```
  - 会自动生成以下几个文件：
  ```
    index.html 入口文件
    README.md 会做为主页内容渲染
    .nojekyll 用于阻止 GitHub Pages 会忽略掉下划线开头的文件
  ```
- 3、本地启动项目
  ```
    docsify serve docs
  ```
- 4、通过 github 发布文档，详细部署可以查看官方文档；

## 官方文档：

[docsify](https://docsify.js.org/#/)

```
https://docsify.js.org/#/

```
