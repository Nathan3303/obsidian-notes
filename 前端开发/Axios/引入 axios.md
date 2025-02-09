# 安装再引入

## 安装

下列安装方式任选一个。

```bash
$ npm install axios
$ bower install axios
$ yarn add axios
$ pnpm add axios
```

## 引入

**CommonJS** 写法：通过 `require` 关键字引入 `axios`

```jsx
const axios = require('axios').default;
```

# CDN 链接直接引入

通过 HTML 标签的方式引入 `axios` 。

```jsx
<script src="https://cdn.jsdelivr.net/npm/axios/dist/axios.min.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
<script src="https://cdn.bootcdn.net/ajax/libs/axios/0.27.2/axios.min.js"></script>
```