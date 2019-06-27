# API Design

## 基本的规则
API是一个最重要前后端交互的接口,而接口的定义一定要清晰,准确,不能有歧义或者指代不清的问题,如一个任务的start和begin两个API,让人一头雾水,不知道啥干啥.  
而这种情况遵循REST的标准来设计RESTful的API就足够了

## 实践问题
实践中基本的模块划分十分有条理,但是有部分牵涉到两个模块的API则需要好好掂量一下放的位置,如用任务查询订单和用订单查询任务,我们则将其分别放入到订单和任务中:毕竟最终查询的结果才是最重要的  
API的文档也要十分清晰,如最基本的URL和method,一句话的描述,提交的数据和返回的数据.诚然..很多时候大家都是要用工具来进行很多次调试才能完成对一个功能的开发.而且有时候修改了逻辑忘记更新文档,就很容易让人迷惑.

# API具体设计

## Session
Auth will check when access some personal info
When fail,it will return ```{status:'session fail'}```

## File
### Upload
POST https://www.volley99.com/file/  
return 200
### Download
GET https://www.volley99.com/file/:id  
return 200 with image  
or return default img

## Render (deprecated)
### get mainpage
GET https://www.volley99.com/


### get testpage
GET https://www.volley99.com/test  

## User Part

### get user self (Need Auth and self)
GET "https://www.volley99.com/users/self/

return
```JSON
{"_id":"5ce554fba2229c3a88b1fc15",
"username":"test1",
"password":"15ds5ad",
"email":"asd@mail.com",
"phone":"13800138000",
"uid":"676271cb-ca17-4fcb-98de-174a21c6b1f7"
```


### Rate someone
POST https://www.volley99.com/users/rating

body:
```JSON
{
  "uid":...
  "rate":...
  "oid":...
}
```


### get user info by uid (future Need Auth)
GET https://www.volley99.com/users/info/:id

return
```
{
  uid
  username
  credit
  phone
  url
}
```

### Check name is avaliable
GET https://www.volley99.com/users/checkname/:name

return
```
True #or
False
```


### get all users (deprecated)
GET https://www.volley99.com/users

return like
```
[{
  "_id":"5ce554fba2229c3a88b1fc15",
  "username":"test1",
  "password":"15ds5ad",
  "email":"asd@mail.com",
  "phone":"13800138000",
  "uid":"676271cb-ca17-4fcb-98de-174a21c6b1f7"
}]
```

### Add new user / Register
POST https://www.volley99.com/users/reg 
```JSON
{
  username:"test",
  password:"123",
  phone:"13800138000",
  email:"example@mail.com"
  nickname:'ass'}
```

return  
True or  
False

### Login
POST https://www.volley99.com/users/login
```JSON
{"username":"test","password":"123"}
```

success return ```{"status": "success"}```  
fail return ```{"status": "error"}```with 200


### Logout
GET https://www.volley99.com/users/logout

No respone


### Edit user
POST https://www.volley99.com/users/update

Update what you want
```
{
  username:'xxx'
}
```

return
True #or
False

### Delete user by id  (Need Auth)
GET https://www.volley99.com/users/delete/:id

return  
True with 204 #or  
False  

### Sign up
签到
GET https://www.volley99.com/users/sign

return -1 or sign up number

## Wallet Part
All need to Auth
### Create wallet
GET "https://www.volley99.com/wallet/create"

### get balance
GET "https://www.volley99.com/wallet/balance"

return int


### charge to self wallet
GET "https://www.volley99.com/wallet/deposit/:amount"

return boolean


### list transaction
GET "https://www.volley99.com/wallet/transaction"  
return list of transaction


### make transaction
POST "https://www.volley99.com/wallet/transaction" 
```JSON
{
  sender:"(uuid)",
  receiver:"(uuid)",
  amount:(int)
}
```
return boolean

### Task Part

### Create task
Need to Auth
POST https://www.volley99.com/task/create
```
{
  "title":"test task",
  "type":"Questionaire",
  "salary":"20",
  "description":"task for test",
  "beginTime":"8-20-2019",
  "expireTime":"8-22-2019",
  "participantNum":"1",
  "tags":"Testing",
  position:"test"
}
```

### Get All Task
GET "https://www.volley99.com/task/all"
Return task list

### Get one task
GET "https://www.volley99.com/task/get/:id"

return info
#or
{status:'error'}


### Query Task By element
POST "https://www.volley99.com/task/query
```bash
{"title":"test task"}

# Allow element
#title
#type
#salary
#description
#beginTime
#expireTime
#participantNum
#tags
#uid
```


### Get created task
GET https://www.volley99.com/task/getCreate/  
Return
```
[{
  ...
}]
```

### Get order task
GET https://www.volley99.com/task/getJjoin/  
Return
```
[{
  ...
}]
```

### Get finish number

Parameter:getTaskbyID

GET "https://www.volley99.com/task/number/:id"


### Select participator
POST "https://www.volley99.com/task/participate"
```
{"tid":"...","uid":"..."}'
```

### Get participator
GET https://www.volley99.com/task/participator/:tid

return [{order}]

### Change task status

Parameter: id: task id
Return: task status

Set to 已结束:
GET "https://www.volley99.com/task/finish/:id"


Set to 进行中:
GET "https://www.volley99.com/task/ongoing/:id"

Set to 未开始:
GET "https://www.volley99.com/task/start/:id"

## Order part(All need auth)
status
```
进行中
候补中
已完成
已关闭
已失效
已评价
```
### create
POST http://www.volley99.com/order/create  
```json
{
  tid:'xxx',
  //message:'xxx',
  status:'success'/'waiting'
}
#成功报名并且已经开启,和成功报名但需要等待确认/启动
```
Return
```
{status:'success'},code 200
#or
{status:'fail'} code 400
```


### Get all order of oneself
GET http://www.volley99.com/order/all  
Return
```json
[{
  oid:x,
  tid:x,
  status:open/end,
  uid:x,
  createTime:x,
  message:'asshole',
  price:1
}]
```

### Get all order of a task
GET http://www.volley99.com/order/bytask/:id  
Return
```json
[{
  oid:x,
  tid:x,
  status:open/end,
  uid:x,
  createTime:x,
  message:'asshole',
  price:1
}]
```

### Get order by id
GET http://www.volley99.com/order/get/:id  
Return
```
order info,code 200
# or
{status:'fail'} code 400
```

### Accomplish one order
完成任务并且拿钱
POST https://www.volley99.com/order/accomplish
```json
{oid:'xxx',finishNumber:'xxx'}
#return
{status:'success'},code 200
# or
{status:'fail'} code 400
```

### Cancel order of oneself
取消某人自己的订单
GET https://www.volley99.com/order/close/:id

```
#return
{status:'success'},code 200
# or
{status:'fail'} code 400
```

### Comment one order by self or by hoster
评论某一订单
POST https://www.volley99.com/order/comment
```
{
  oid:'xx',
  comment:'xx',
  credit:0
}
#return
{status:'success'},code 200
# or
{status:'fail'} code 400
```

### From backup to formal member
备胎转正
GET https://www.volley99.com/order/turnbegin/:id  
various of ```{status:'xxx'}```

### Get waiting list
GET https://www.volley99.com/order/waitinglist/:id

return list of order

### Set order pending
GET https://www.volley99.com/order/turnpending/:id

## Message part(All need auth)

** 评论的类型为"comment",报名的话type "enrollment" **

### Get a receiver's msg list
Get http://www.volley99.com/msg/list

### Create a comment
POST http://www.volley99.com/msg/comment
```
{uid:"...",
msg:"test"}
```

uid is recerver's id

### Create a Massage

POST http://www.volley99.com/msg/create

```json
 {uid:"...",type:"comment",msg:"test"}
```

### Create an enrollment
POST http://www.volley99.com/msg/enroll
```
{uid:"...",
msg:"test"}
```





Test Module
=====
use postman team


**Tested API:**

- All of User
- All of Wallet


Doc
===
## KOA
[Example](https://github.com/koajs/examples)

## MONK
[Monk](https://automattic.github.io/monk/docs/GETTING_STARTED.html)

## Hapi Joi
[Link](https://github.com/hapijs/joi)
[API](https://github.com/hapijs/joi/blob/v15.0.3/API.md)

## Chinese Tutorials
[This](https://chenshenhai.github.io/koa2-note/)

## Koa session
[This](https://github.com/koajs/session)

## Koa Passport
[This](https://github.com/rkusa/koa-passport)

## Auth example
[This](https://mherman.org/blog/user-authentication-with-passport-and-koa/)
