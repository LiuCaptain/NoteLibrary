# 解决 React 项目报错 Property 'toBeInTheDocument' does not exist on type 'JestMatchers<HTMLElement>'.

1. ###### 项目报错复现

   我使用 `create-react-app` 创建一个全新的 React 应用程序，生成的项目目录结构如下：

   <img src="https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e43484c105944b2d872e739b24f8ff34~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=242&h=196&s=8361&e=png&b=23272d"  />

   这里可以先说一下，项目默认是使用 `npm` 作为包管理器；也可以看到项目路径 `node_modules/@types` 下存在 `testing-library__jest-dom` 类型文件

   ![Snipaste_2024-06-26_11-37-17](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/b8bd915539d8416e88dd86398c59a127~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=243&h=266&s=10015&e=png&b=22262c)

   此时在终端中执行 `npm start` 是可以正常启动项目。

   

   但我在项目开发中比较喜欢使用 `pnpm` 作为包管理器，所以我把 `node_modules` 文件夹和 `package-lock.json` 文件删除，然后在终端中执行 `pnpm i` 命令重新安装依赖，再执行 `pnpm start` 启动项目，项目启动后就报了 **Property 'toBeInTheDocument' does not exist on type 'JestMatchers<HTMLElement>'** 这个错误

   ![Snipaste_2024-06-26_14-39-11](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/41f820dcbe5e4c33bedbc7236915122c~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=827&h=338&s=31367&e=png&b=180608)

   至此我很疑惑，因为我没有改动项目源码，经过仔细分析看起来是类型文件问题，于是我打开 `node_modules/@types` 文件夹，发现里面没有 `testing-library__jest-dom` 类型文件

   ![image-20240626143730188](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d74fa7c2ae0b4857a69fafa91660f670~tplv-k3u1fbpfcp-jj-mark:0:0:0:0:q75.image#?w=262&h=212&s=6996&e=png&b=23272d)

2. ###### 项目报错解决

   查阅了官方一些资料，发现在 pnpm v7 版本官方已经对 types 文件不再提升到 `node_modules` 的根，于是我执行 `pnpm add @types/testing-library__jest-dom`，再重新启动项目发现还是报错，大胆猜测应该是 `@types/testing-library__jest-dom` 里面的依赖没能正确依赖，果断卸载 `@types/testing-library__jest-dom`。

   

   继续查阅官方资料，了解到可以通过在 `.npmrc` 中配置 `public-hoist-pattern[]=*types*` 将 types 全部提升到 `node_modules` 根目录，这样再重新执行 `pnpm i` 安装依赖后，依赖之间的互相依赖就可以正常依赖上了；但是我希望提升的是 @types 而不是 types,所以我最终使用`@types*` 而不是 `*types*`来避免错误地提升其他非类型依赖包；另外，`pnpm` 的 `public-hoist-pattern[]` 有默认配置 **`['*eslint*', '*prettier*']`**，并且在 `.npmrc` 中设置它会覆盖默认配置，所以最终 `.npmrc` 是：

   ```ini
   public-hoist-pattern[]=*eslint*
   public-hoist-pattern[]=*prettier*
   public-hoist-pattern[]=@types*
   ```

   > 注意，在项目中添加 `.npmrc` 文件后需要执行 `pnpm i` 重新安装依赖


