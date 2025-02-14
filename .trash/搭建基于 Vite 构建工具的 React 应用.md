
本文不使用官方的 Create-react-app 脚手架直接生成，而是通过手工安装和配置各个子模块来搭建出一个 React 应用，并且使用 TypeScript 作为开发语言。

通过手动安装的方式，可以了解 React 项目的架构方案以及构建流程，了解各个子模块之间是如何相互引用的。

## 包管理工具

本文将使用 pnpm 作为包管理工具。pnpm 作为 npm 的 “升级版”，解决了**安装速度慢**、**依赖解析问题**以及**磁盘空间浪费**等问题。

### 安装 pnpm

```shell

npm install -g pnpm

```

安装完成后可以需要重新打开一个终端才能够使用 pnpm 。

## 初始化项目目录

### 创建工程目录

```shell

mkdir <your_project_name>

```

### 初始化工程

```shell

pnpm init

```

## 搭建一个最小可运行的 React 应用

### 安装 React 和 Vite 的相关依赖

```shell

# 安装 react 核心包

pnpm add react react-dom

  

# 安装构建工具和相关插件

pnpm add vite @vitejs/plugin-react -D

```

### 创建 Vite 的配置文件

在项目根目录创建文件 `vite.config.js`

```js

import { defineConfig } from "vite";

import react from "@vitejs/plugin-react";

  

export default defineConfig({

    plugins: [react()],

});

```

### 创建入口文件 index.html

在项目根目录创建文件 `index.html`

```html

<!DOCTYPE html>

<html lang="en">

    <head>

        <meta charset="UTF-8" />

        <meta name="viewport" content="width=device-width, initial-scale=1.0" />

        <title>My React App</title>

    </head>

    <body>

        <div id="root"></div>

        <script type="module" src="/src/App.js"></script>

    </body>

</html>

```

### 创建 React App 组件

在 `src` 目录下创建组件文件 `App.js`

```js

import React from "react";

import ReactDOM from "react-dom/client";

  

ReactDOM.createRoot(document.getElementById("root")).render(

    <React.StrictMode>

        <h1>Hello World!</h1>

    </React.StrictMode>

);

```

### 增加启动命令

修改根目录的 `package.json` 文件，增加项目的启动命令

```json

"scripts": {

  "dev": "vite",

  "build": "vite build",

  "preview": "vite preview"

},

```

### 启动开发服务器

```shell

pnpm dev

```

到此一个最小可运行的 React 应用就搭建完成了，接下来就是在这个应用的基础上集成其他的包，集成一些工具和规范，使得整个应用架构更饱满，更符合现代前端工程。

## 集成 TypeScript

### 安装 TypeScript 以及类型依赖

```shell

pnpm add typescript @types/react @types/react-dom vite-plugin-typescript tslib -D

```

### 创建 TypeScript 的配置文件

在项目根目录创建文件 `tsconfig.json`

```json

{

  "compilerOptions": {

    "target": "es5",

    "lib": ["dom", "dom.iterable", "esnext"],

    "allowJs": true,

    "skipLibCheck": true,

    "strict": true,

    "forceConsistentCasingInFileNames": true,

    "noEmit": true,

    "esModuleInterop": true,

    "module": "esnext",

    "moduleResolution": "node",

    "resolveJsonModule": true,

    "isolatedModules": true,

    "jsx": "preserve",

    "incremental": true,

    "baseUrl": ".",

    "outDir": "dist",

    "rootDir": "src"

  },

  "include": ["./src/**/*"],

  "exclude": ["node_modules"]

}

```

### 修改 Vite 的配置文件

在 `vite.config.js` 文件中添加对 TypeScript 的支持

```js

import { defineConfig } from "vite";

import react from "@vitejs/plugin-react";

import typescript from "vite-plugin-typescript";

  

export default defineConfig({

    plugins: [

        react({
          include: "**/*.{js,jsx,ts,tsx}",

        }),

        typescript({

            tsconfig: "./tsconfig.json",

        }),

    ],

});

```

### 增加类型检查命令

修改 `package.json` 文件，增加类型检查的命令

```json

"scripts": {

  "dev": "vite",

  "build": "vite build",

  "preview": "vite preview",

  "typecheck": "tsc --noEmit"

},

```

  

### 创建一个 TypeScript React 组件

  

创建文件 `src/components/Hello.tsx`

  

```tsx

import React from "react";

  

interface HelloWorldProps {

    name: string;

}

  

export default function ({ name }: HelloWorldProps) {

    return <h1>Hello {name}!</h1>;

}

```

  

### 修改 React App 文件内容及后缀

  

1. 将 `src/App.jsx` 替换为 `src/App.tsx`

2. 修改 `src/App.tsx`，引入并使用上面创建的 `src/components/Hello.tsx` 组件。

  

```ts

  

```

  

### 运行类型检查

  

```shell

pnpm typecheck

```

  

### 启动开发服务器

  

```shell

pnpm dev

```

  

到此 TypeScript 的集成工作已经全部完成。

  

## 集成 ESLint 和 Prettier

  

### 安装相关依赖

  

```shell

pnpm add eslint prettier typescript-eslint @eslint/js @typescript-eslint/parser @typescript-eslint/eslint-plugin -D

```

  

### 创建 ESLint 的配置文件

  

在项目根目录创建 `eslint.config.mjs`

  

```mjs

import pluginJs from '@eslint/js';

import tseslint from 'typescript-eslint';

  

export default [

    {

        files: ['src/*.{js,jsx,ts,tsx,json}'],

    },

    pluginJs.configs.recommended,

    ...tseslint.configs.recommended,

    {

        languageOptions: { parserOptions: { parser: tseslint.parser } },

        rules: {

            '@typescript-eslint/no-explicit-any': 'off',

        },

    },

    {

        ignores: ['node_modules/', 'dist/', '*.test.js'],

    },

];

```

  

### 创建 Prettier 的配置文件

  

在项目根目录创建 `.prettierrc.json`

  

```json

{

  "printWidth": 80,

  "tabWidth": 4,

  "useTabs": false,

  "semi": true,

  "singleQuote": true,

  "trailingComma": "all",

  "bracketSpacing": true,

  "jsxBracketSameLine": true,

  "arrowParens": "avoid",

  "proseWrap": "preserve"

}

```

  

### 创建 ESLine 和 Prettier 的执行命令

  

修改 `package.json`，增加 `lint`、 `format` 以及 `precommit` 命令

  

```json

"scripts": {

    "dev": "vite",

    "build": "vite build",

    "preview": "vite preview",

    "lint": "eslint",

    "format": "prettier src/ --write",

    "precommit": "pnpm lint && pnpm format"

}

```

  

### 运行新增的命令

  

分别运行 `lint`、 `format` 以及 `precommit` 命令，测试效果

  

```shell

pnpm lint

pnpm format

pnpm precommit

```

  

至此 ESLint 以及 Prettier 规范工具的集成就完成了。