# 前端工程化之规范仓库提交记录（husky）

## 所需要用到的库：

- `husky`：在项目中通过 `husky` 可以便捷地添加 Git hooks（core.hooksPath），`husky ` 支持[所有的 Git 钩子](https://git-scm.com/docs/githooks)。
- `lint-staged`：Run linters against staged git files and don't let 💩 slip into your code base!  `--引自官方`
- `commitlint`：帮助团队遵守提交约定。通过支持 `npm` 安装的配置，它使提交约定的共享变得容易。
- `commitizen`：基于 Node.js 的 `git commit` 命令行工具，辅助生成标准化规范化的 commit message。
- `cz-git`：一款工程性更强，轻量级，高度自定义，标准输出格式的 [commitizen](https://github.com/commitizen/cz-cli) 适配器。

### 1、husky 安装与配置（*monorepo*）

- 安装：

  ```
  pnpm add husky -D
  ```

- 启用Git钩子

  ```
  npx husky install
  ```

- 在项目每次 `install` 后都执行 `husky install`，利用 `prepare`

  ```
  pnpm pkg set scripts.prepare="husky install"
  ```

- 配置 Git hooks（`pre-commit`、`commit-msg`）

  ```
  npx husky add .husky/...
  ```

### 2、lint-staged 安装与配置（*monorepo*）

- 安装

  ```
  pnpm add lint-staged -D
  ```

- 配置文件

  ```yaml
  # .lintstagedrc.yml
  
  {
    "*.{js,jsx}": ["prettier --write", "eslint --fix"],
    "*.{ts,tsx}": ["prettier --write", "eslint --fix"],
    "*.vue": ["prettier --write", "eslint --fix"],
    "*.{less,css,html}": ["prettier --write"],
    "*.{md,json}": ["prettier --write"],
  }
  ```
  

### 3、prettierrc 配置

```js
// .prettierrc

{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "printWidth": 100
}
```

### 4、commitlint 安装与配置

- 安装

  ```
  pnpm add @commitlint/config-conventional @commitlint/cli -D
  ```

- 配置执行 commitlint

  ```
  npx --no -- commitlint --edit ${1}
  ```

- 配置文件

  ```js
  // commitlint.config.js
  
  module.exports = { extends: ['@commitlint/config-conventional'] };
  ```

### 5、commitizen 安装与配置

- 安装

  ```
  pnpm add commitizen -D
  ```

- 配置文件

  ```js
  // .czrc
  
  {
    "path": "cz-git"
  }
  ```

### 6、cz-git 安装与配置

- 安装

  ```js
  pnpm add cz-git -D
  ```

- 调用 `cz` 命令

  ```json
  // package.json
  
  "scripts": {
    "commit": "git cz",
  },
  ```

- 配置文件

  ```js
  // commitlintrc.js
  
  /** @type {import('cz-git').UserConfig} */
  const fs = require('fs');
  const path = require('path');
  const packages = fs.readdirSync(path.resolve(__dirname, 'packages'));
  const apps = fs.readdirSync(path.resolve(__dirname, 'apps'));
  
  module.exports = {
    rules: {
      'body-leading-blank': [2, 'always'],
      'footer-leading-blank': [1, 'always'],
      'header-max-length': [2, 'always', 108],
      'subject-empty': [2, 'never'],
      'type-empty': [2, 'never'],
      'subject-case': [0],
      'scope-enum': [2, 'always', [...packages, ...apps, 'turbo']],
      'type-enum': [
        2,
        'always',
        ['feat', 'fix', 'docs', 'style', 'refactor', 'perf', 'test', 'build', 'ci', 'chore', 'revert']
      ]
    },
    extends: ['@commitlint/config-conventional'], // TODO ********************************************
    prompt: {
      useEmoji: true,
      scopes: [...packages, ...apps],
      messages: {
        type: '选择你要提交的类型 :',
        scope: '选择一个提交范围（可选）:',
        customScope: '请输入自定义的提交范围 :',
        subject: '填写简短精炼的变更描述 :\n',
        body: '填写更加详细的变更描述（可选）。使用 "|" 换行 :\n',
        breaking: '列举非兼容性重大的变更（可选）。使用 "|" 换行 :\n',
        footerPrefixesSelect: '选择关联issue前缀（可选）:',
        customFooterPrefix: '输入自定义issue前缀 :',
        footer: '列举关联issue (可选) 例如: #31, #I3244 :\n',
        confirmCommit: '是否提交或修改commit ?'
      },
      types: [
        {
          value: 'feat',
          emoji: ':sparkles:',
          name: 'feat:     ✨  功能新增 | 一切都是新的，这是好的开始。'
        },
        {
          value: 'fix',
          emoji: ':bug:',
          name: 'fix:      🐛  缺陷修复 | 死吧，虫子！'
        },
        {
          value: 'docs',
          emoji: ':memo:',
          name: 'docs:     📝  文档变动 | 这个文档有人能看懂吗？'
        },
        {
          value: 'style',
          emoji: ':lipstick:',
          name: 'style:    💄  样式变动 | 人靠衣装码靠样装。'
        },
        {
          value: 'refactor',
          emoji: ':recycle:',
          name: 'refactor: ♻️   代码重构 | 现在只有神能帮我重构了。'
        },
        {
          value: 'perf',
          emoji: ':zap:',
          name: 'perf:     ⚡️  性能提升 | 这次提升足足快了1秒！'
        },
        {
          value: 'test',
          emoji: ':white_check_mark:',
          name: 'test:     ✅  测试相关 | 试试就逝世。'
        },
        {
          value: 'build',
          emoji: ':package:',
          name: 'build:    📦️  构建相关 | 可以帮我完成这张拼图吗？'
        },
        {
          value: 'ci',
          emoji: ':ferris_wheel:',
          name: 'ci:       🎡  持续集成 | 全程自动化！'
        },
        {
          value: 'chore',
          emoji: ':hammer:',
          name: 'chore:    🔨  其他修改 | 剩下的事情就交给我。'
        },
        {
          value: 'revert',
          emoji: ':rewind:',
          name: 'revert:   ⏪️  代码回滚 | 只想回到过去。'
        }
      ],
      allowCustomScopes: true,
      customScopesAlign: 'bottom',
      customScopesAlias: 'custom',
      allowEmptyScopes: false,
      emptyScopesAlias: 'empty'
    }
  };
  
  ```

  

### 7、测试 eslint 报错时是否能够提交

### 8、报错问题

解决项目中出现 Parsing error: Cannot find module 'next/babel'

- 安装 `babel-eslint`

  ```
  pnpm add -D babel-eslint -F eslint-config-custom
  ```

- 配置公共 `eslint`

  ```js
  {
    parser: "babel-eslint",
  }
  ```

