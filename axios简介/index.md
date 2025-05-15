# axios简介


# axios

![axios](/images/axios.png)

axios 对原生 ajax 进行了封装，简化书写，快速开发，用于发送**异步**请求。

[查看 axios docs](https://axios-http.com/zh/docs/intro)

## 定义

Axios 是一个基于 _[promise](https://javascript.info/promise-basics)_ 网络请求库，作用于[`node.js`](https://nodejs.org/) 和浏览器中。 它是 _[isomorphic](https://www.lullabot.com/articles/what-is-an-isomorphic-application)_ 的(即同一套代码可以运行在浏览器和 node.js 中)。在服务端它使用原生 node.js `http` 模块, 而在客户端 (浏览端) 则使用 XMLHttpRequests。

## 用例

发起一个 `GET` 请求

```js
const axios = require("axios");

// 向给定ID的用户发起请求
axios
  .get("/user?ID=12345")
  .then(function (response) {
    // 处理成功情况
    console.log(response);
  })
  .catch(function (error) {
    // 处理错误情况
    console.log(error);
  })
  .finally(function () {
    // 总是会执行
  });

// 上述请求也可以按以下方式完成（可选）
axios
  .get("/user", {
    params: {
      ID: 12345,
    },
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  })
  .finally(function () {
    // 总是会执行
  });

// 支持async/await用法
async function getUser() {
  try {
    const response = await axios.get("/user?ID=12345");
    console.log(response);
  } catch (error) {
    console.error(error);
  }
}
```

> **注意:** 由于`async/await` 是 ECMAScript 2017 中的一部分，而且在 IE 和一些旧的浏览器中不支持，所以使用时务必要小心。

发起一个 `POST` 请求

```js
axios
  .post("/user", {
    firstName: "Fred",
    lastName: "Flintstone",
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

