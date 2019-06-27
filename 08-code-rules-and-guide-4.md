# 安装包与前后端部署说明

## 服务器部署说明

**源码地址**：[server仓库](https://github.com/2019swsad/server)

**环境要求**：node.js v10.16.0

**证书**：本项目使用Https进行加密，部署服务器需要ssl证书才能成功部署。证书恕不提供，需要回落http或自行设置证书目录请在app/app.js修改。如果您决定回落http，为了成功测试前端请在微信上开启免验证的功能。

**DB要求**：安装MongoDB并运行。

### 部署步骤

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

## 前端部署说明

### 安装包
由于本项目是基于微信小程序平台开发的Web应用，不需要通过安装包进行安装。然而，**由于小程序尚未完成微信的审核，在您检验软件成果的时候可能需要劳烦您联系本小组组长(姚同学:组队信息中的QQ号/yaojw7@mail2.sysu.edu.cn)，组长将会把您加入到小程序测试名单中，此后便能通过微信访问本程序。**

除此以外，您还可以下载本项目的源代码，通过微信开发者工具在手机上进行运行测试，具体的部署说明如后文。

### 源代码
您可以在此[链接](https://github.com/2019swsad/AngryCat)下载项目的微信小程序源代码。

### 安装部署
- 打开[微信web开发者工具](https://developers.weixin.qq.com/miniprogram/dev/devtools/download.html)，并点击图中按钮
  ![1](https://github.com/2019swsad/Software-Engineering-Experiment-Course-Document/blob/master/assets/Deployment/1.jpg)
- 点击"Import Project"选项卡，选择项目代码包所在的文件夹，并点击下方的“Test Account”按钮，随后便可以点击“Import”按钮打开项目
  ![2](assets/Deployment/2.jpg)
- 打开项目后，点击"Preview"按钮，选择“Scan QR Code With WeChat”并用手机微信扫描下方二维码，在手机上对程序进行测试。**我们强烈建议您使用手机进行测试，微信开发者工具提供的模拟器并不稳定，时常出现莫名其妙的BUG**
  ![3](assets/Deployment/3.jpg)

## 常见问题
1. 在模拟器上测试时，**偶尔会出现“无法选中输入框”或“输入过程中失焦”的情况**。这是微信Web开发者工具的BUG，只需要多次点击输入框(点击时，请点击输入框上方位置以便选中)
2. 在模拟器上测试时，**偶尔会出现界面卡死的情况**，需要重新编译

**再次建议使用手机测试，不要用模拟器**

### 常见问题
