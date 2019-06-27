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

随着前端技术的不断演进，目前市面上的移动端产品有像大前端转换的趋势。开发一个产品主流上大致有5种开发模式，包括 Native APP 原生应用、WebAPP 网页应用、Hybrid App 混合式开发、ReactNative、轻应用小程序。开发一款 APP，如果注重性能，不需要过多的动态内容，可以选择原生应用来开发。如果性能要求不高，只是需要点开即用，那么 WebAPP 即可满足。如果这两种要求都必须满足，那么 Hybrid App 混合式的开发是一种折中的方法，将两者优势互补，满足大部分的需求。近几年，出现了类似 ReactNative 和小程序这种深度定制的开发模式，开发只需要写 Javascript 就可以产出性能可与原生媲美的产品，通过脚本语言面向开发者，实际跑的确是的原生组件。
作为学校中的一个初创团队，微信小程序可以说很自然就成为了我们的首选。作为开发者，我们肯定希望一个新的开发模式最好能用现在的知识去开发，而不是学习新的语言，小程序开发没有去造新的语言，使用前端开发的知识基础就能上手开发，小程序官方提供了详细的开发文档和资料，并且提供了一套开发工具供开发者使用，一直在迭代维护升级中，小程序提供了丰富的接口帮助我们快速的开发业务。减少了很多开发成本。

作为运营人员，小程序提供了一整套的运营体系，包括多维的入口，数据分析工具，各项数据都能很好的展现给运营人员，再加上微信的流量红利，能够减少很多运营者获取用户的成本。

程序借鉴了很多前端开发的技术理念，它用React实现了“视觉组件”，它用CMD的require作为面向对象的．JavaScript，用Vue实现了标签式逻辑与数据绑定。

 微信小程序定义了自己专有的模型，吸收了主流前端开发中数据、样式、控制逻辑分离的理念，剔除了烦琐的关联配置，并且从规范上要求每个“页面”需要有同名的四个文件：index．js、index．json、index．wxml、index．WXSS，各司其职。其中js文件采用标准的JavaScript语法规范，用于逻辑操作；json文件基于．ISON语法规范，用于页面文件的配置；wxml文件基于xML语法规范，用于页面视觉组件的描述；而WXSS继承了标准的CSS语法，用于wXML组件样式的定义。

小程序的页面加载基于本地，不需要通过频繁的服务端请求来实现，所有的页面跳转都不需要通过服务端交互。这将使小程序的执行效率大大提高，比服务号、企业号等基于H5的Web应用模式有更好的用户体验，操作流畅度与反应速度也会更好。这也意味着在没有网络连接的环境下也可以使用微信小程序。结合本地存储，小程序可以满足暂时断网或网络情况较差的场景需求，这是微信服务号和Web服务都无法实现的。


总体来说，微信小程序的价值，将会体现在下面几个方面：

 (1)对于开发者，小程序因为兼容．1avaScript和xML、css语法规范，这将会使开发门槛更低，开发一个程序将会变得更简单。在小程序平台上线初期，会产生一个密集的应用分发高潮，并将持续半年到一年的时间，开发者将能够借助微信平台获得较大的流量，这将比App的流量获取更加容易，也能让营销成本变得更低。

(2)对用户来说，小程序因为它的即开即用特性，将会减轻手机的应用压力，避免资源浪费。微信小程序的审核机制会比App更为严格，应用程序将很难获取到用户的敏感权限，这将使用户使用手机更为安全，小程序的获取渠道将更多地集中在微信上，降低了用户获取应用的时间成本。

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
