# 银行API

> 使用[Node.js](https://nodejs.org) + [Express](https://expressjs.com/)构建的银行 API 。

该 API 已为您构建，而不是练习的一部分。

但是，如果您有兴趣了解如何构建这样的 API，可以关注本系列视频：https://aka.ms/NodeBeginner（视频 17 到 21 涵盖了这个确切的 API）。

您还可以查看此交互式教程：https://aka.ms/learn/express-api

## 运行服务器

确保您已安装 [Node.js](https://nodejs.org) 

1. Git 克隆此存储库 [The Web-Dev-For-Beginners](https://github.com/microsoft/Web-Dev-For-Beginners).
2. 打开终端并导航到`Web-Dev-For-Beginners/7-bank-project/api`文件夹
2. 运行`npm install`并等待软件包安装（可能需要一段时间，具体取决于您的互联网连接质量）。
3. 安装完成后，运行`npm start`即可。

服务器应该开始侦听端口`5000`。 该服务器将与主银行应用程序服务器终端一起运行（监听端口`3000`），请勿关闭它。

> 注意：所有条目都存储在内存中并且不会持久保存，因此当服务器停止时所有数据都会丢失。

## API详情

路由                                          | 描述
---------------------------------------------|------------------------------------
GET    /api/                                 | 获取服务器信息
POST   /api/accounts/                        | 创建一个帐户，例如： `{ user: 'Yohan', description: 'My budget', currency: 'EUR', balance: 100 }`
GET    /api/accounts/:user                   | 获取指定账户的所有数据
DELETE /api/accounts/:user                   | 删除指定账户
POST   /api/accounts/:user/transactions      | 添加交易，例如：`{ date: '2020-07-23T18:25:43.511Z', object: 'Bought a book', amount: -20 }`
DELETE  /api/accounts/:user/transactions/:id | 删除指定交易
