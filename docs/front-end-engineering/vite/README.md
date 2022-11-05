## vite init

### 创建Vite2项目

- 使用npm:

```bash
 npm init @vitejs/app

```

- 按指示指定项目名称和目标，或直接指定：

```bash
npm init @vitejs/app my-vue-app --template vue
```

- 支持的模板预设包括：

  - vue
  - vue-ts
  - react
  - react-ts
  - ......

- 命令行接口

```bash

{
  "scripts": {
    "dev": "vite", // 启动开发服务器
    "build": "vite build", // 为生产环境构建
    "serve": "vite preview" // 本地预览生产构建产物
  }
}

 npm install

 npm run dev

 // OR 

 yarn

 yarn dev
```

### vite 具有以下特点

1. 快速的冷启动
2. 即时热模块更新（HMR，Hot Module Replacement）
3. 真正按需编译：Vite是在推出Vue 3的时候开发的，目前仅支持Vue 3.x，这意味着与Vue 3不兼容的库也不能与Vite一起使用。

### vite.js中文网

> `https://www.pipipi.net/vite/guide/`

### 推荐【Young村长】干货

- [备战2021：Vite2项目最佳实践 文章](https://juejin.cn/post/6924912613750996999#heading-2)
- [备战2021：Vite2项目最佳实践 视频](https://www.bilibili.com/video/BV1vX4y1K7bQ)
