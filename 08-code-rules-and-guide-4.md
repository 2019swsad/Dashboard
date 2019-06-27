# 服务器部署说明

**源码地址**：[server仓库](https://github.com/2019swsad/server)

**环境要求**：node.js v10.16.0

**证书**：本项目使用Https进行加密，部署服务器需要ssl证书才能成功部署。证书恕不提供，需要回落http或自行设置证书目录请在app/app.js修改。

**DB要求**：安装MongoDB并运行。

## 部署步骤

1. 下载server仓库中的源码。

2. 进入项目目录，在环境拥有cnpm的工具时，输入：

   ```
   $ cnpm i
   ```
   否则，输入：

   ```
   $ npm install
   ```
   用于安装项目所需module。

3. 输入：

   ```
   node ./index.js
   ```
   运行server。
