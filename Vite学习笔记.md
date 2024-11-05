1. Vite 依赖预构建过程中主要做了什么？

   - 在开发阶段中，Vite 的开发服务器将所有代码视为原生 ES 模块。所以 Vite 必须先将以 CommonJS 或 UMD 形式提供的依赖项转换为 ES 模块

   - Vite 将那些具有许多内部模块的 ESM 依赖项转换为单个模块，减少网络请求的数量，提高后续页面的加载性能

   - Vite 将预构建的依赖项缓存到 `node_modules/.vite` 中，避免每次启动开发服务器时重复构建

     > - 在依赖预构建中 Vite 使用 `esbuild` 将以 CommonJS 或 UMD 形式提供的依赖项转换为 ES 模块。在生产构建中，将使用 `@rollup/plugin-commonjs`。
     > - 在依赖预构建中 Vite 使用 `esbuild` 将有许多内部模块的 ESM 依赖项转换为单个模块

2. optimizeDeps.include 和 optimizeDeps.exclude 的作用

   - optimizeDeps.include：如果依赖项很大（包含很多内部模块）或者是 CommonJS，那么可以显式地将它加入 include，这样 Vite 会在服务器启动时对它进行预构建，减少延迟。

   - optimizeDeps.exclude：如果依赖项很小，并且已经是有效的 ESM（ES 模块），那么可以将它排除在预优化之外，直接让浏览器加载它，从而减少打包时间。

     > 将依赖手动添加到 `optimizeDeps.include`，可以让 Vite 在启动时预先打包这些依赖，而不是等到运行时再打包。这样可以加快开发服务器的启动过程，避免因插件转换造成的额外打包开销。