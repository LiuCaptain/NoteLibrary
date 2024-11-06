# 常用的npm镜像源和配置npm、pnpm、yarn的registry

1. ###### 常用的 npm 镜像源

   - npm 官方源：[https://registry.npmjs.org](https://registry.npmjs.org)

   - 阿里云镜像源：[https://registry.npmmirror.com](https://registry.npmmirror.com)
   - 腾讯云镜像源：[http://mirrors.cloud.tencent.com/npm](http://mirrors.cloud.tencent.com/npm)

2. ###### 配置 npm、pnpm、yarn 的 registry

   - npm

     查看 npm 的 registry

     ```sh
     npm config get registry
     ```

     配置 npm 的 registry

     ```sh
     npm config set registry=https://registry.npmjs.org
     ```

   - yarn

     查看 yarn 的 registry

     ```
     yarn config get registry
     ```

     配置 yarn 的 registry

     ```sh
     yarn config set registry=https://registry.npmjs.org
     ```

   - pnpm

     查看 pnpm 的 registry

     ```
     pnpm config get registry
     ```

     配置 pnpm 的 registry

     ```sh
     pnpm config set registry=https://registry.npmjs.org
     ```