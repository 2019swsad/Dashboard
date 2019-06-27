# Software Design

## 技术选型
- 后端采用基于 Nodejs 的 Koa 作为主要的服务端框架,采用 Mongodb 作为数据库,其最重要的特点是十分明晰的模块化设计.基于中间件的工作方式能形成一种链式的处理过程,十分适合 Web 应用这种每个请求都有相同点,也存在不同点的请求,这样能拆分请求,流水线化处理.
- 还有就是 mongodb 的非关系型数据库适合快速开发,而不用考虑到表格的问题.使用类 MySQL 的数据库需要考虑表格的设计,但是 mongodb 的不定类型就可以很好地适应迭代开发中对数据属性的增添,避免了迭代时需要对表格进行兼容处理.而且 mongodb 能直接读写 JSON 对象而不用进行翻译转换,这样也提高了代码的可读可维护性.  
- 而 Koa 这个后端框架虽然看起来有点简陋,但是其能兼容比较成熟的 Express 的部件,也能直接使用 Nodejs 的模块,就十分容易进行功能拓展. Koa 的最重要一点是明晰的处理流程,能让请求十分清楚地表现出来和进行级联地处理,其处理像堆栈一样,中间能插入许多符合标准的中间件,这就相当于一种"糖".而这种结构显著提高了其鲁棒性和可靠性,如在app/app.js中,就可以完整体会到这种语法糖的优点如:
```javascript
app = new Koa();
require('koa-ctx-cache-control')(app)
app.use(koaBody({ multipart: true }));
app.keys=['hihihi']
let options={
    key:fs.readFileSync('./crt/server.key'),
    cert:fs.readFileSync('./crt/server.pem')
}
app.use(err);
app.use(enforceHttps({port:443}))
app.use(views('app/views/',{extension:'ejs'}))
app.use(session({}, app));
app.use(bodyParser())
app.use(passport.initialize())
app.use(passport.session())
app.use(router());
```
这样的结构中就是完整由语法糖粘合,各种模块,不管是自己写的亦或者是其它引入的模块,都能很好地一起工作

- 再如
```JavaScript
user = await personDB.find({ username: username, password: password }).then((doc) => { return doc })
```
这样直接采用现有数据来查询,直接得到数据对象,就能很简单地来增删查改



## 架构设计
 
- 后端架构采用比较成熟的Koa开发结构,采用一个完全API化的后端,因为所有前端的页面设计都是承载在微信小程序上,故后端不需要解决静态文件的推送,只需要处理各种逻辑的处理,如管理用户,管理订单,管理任务.  
这样的架构就能顺理成章地让某个逻辑的代码落到某个类的的 controller 中,而这个 controller 暴露出其路由,由上层的初始化层进行综合和启动,并插入一些中间件去处理.而 controller 中只包含了每个API所需要的逻辑处理函数,而部分不和API产生直接关联的函数或者中间件,如数据库的初始化,钱包的操作函数,验证请求的       Session 的中间件,这些都放在了对应的 helper 中,这样避免了逻辑混乱和导致非公开函数被恶意调用
- 如路由部分则是
```JavaScript
const Router = require('koa-router'),
    combineRouters =require('koa-combine-routers'),
    {userRouter} = require('../controllers/userController'),
    walletRouter=require('../controllers/walletController'),
    taskRouter=require('../controllers/taskController'),
    orderRouter=require('../controllers/orderController'),
    indexRouter=require('../controllers/indexRender'),
    fileRouter=require('../controllers/fileController'),
    msgRouter=require('../controllers/msgController');



const router=combineRouters (
    userRouter,
    indexRouter,
    walletRouter,
    taskRouter,
    orderRouter,
    msgRouter,
    fileRouter
)
module.exports = router;
```
- 将所有的路由都在各自 Controller 中生成,再进行导出在一个总路由中,可以使得各模块能分别维护而在基本的物理意义上不互相影响.

- 部分 helper 则是将所有的逻辑先打包,再统一进行暴露,避免了数据库的暴露,从而降低风险
```JavaScript
const date = require("silly-datetime")

function getNow() {
    return date.format(new Date(), 'YYYY-MM-DD HH:mm:ss')
}
function isEarly(time1, time2) {
    t1 = new Date(time1)
    t2 = new Date(time2)
    return t1.getTime() < t2.getTime()
}
module.exports = { getNow, isEarly }
```
- 简单的一个helper就是这种设计方案,将其逻辑全部包含在内而不暴露在外,这样也减少了相互引用导致的编译问题.

## 模块划分
我们把功能划分为:用户,钱包,任务,订单,头像,信息,所有的模块代码都在app/controller/中, 对应的非线上代码则在app/helper中

- 用户(userController.js):处理用户注册,登录,验证,存储用户信息
- 钱包(walletController.js):钱包分为用户钱包和任务钱包,其功能基本一样,主要有转账,充值,记录每笔交易信息
- 任务(taskController.js):用户发布任务,交由系统处理,等待人来完成其任务,故有任务的发布,发布后的人员处理,完成任务和对应的订单
- 订单(orderController.js):用户接收对应任务所产生的订单,订单中包含交易的信息,所以有订单的创建,订单修改,订单关闭,订单评价等功能
- 信息(msgController.js):对应的任务处理都会出发其信息钩子,产生信息,最终让对应用户接收信息
- 头像:处理头像的上传下载
## Object-oriented programming (OOP)
在OOP中最显著的就是将许多东西都对象化,特别是重要数据和复杂一点的数据,都采取了对象化,如:用户,订单,任务,钱包,转账等,都采用了JSON的对象来进行包装,包装后就能直接形成能读能写,也能继续修改,并且能直接和前端进行通讯.  
如在contorller/userController.js中的user对象设计能参考Joi的检查schema,可以看到每个对象都是需要包含5个必须的属性

```JavaScript
const userRegSchema = Joi.object().keys({
    username: Joi.string().alphanum().min(3).max(30).trim().required(),
    password: Joi.string().regex(/^[a-zA-Z0-9]{3,30}$/).required(),
    email: Joi.string().email({ minDomainSegments: 2, }).required(),
    phone: Joi.string().regex(/^[0-9]{11}$/).required(),
    nickname: Joi.string().min(2).max(20).trim().required()
});
```
