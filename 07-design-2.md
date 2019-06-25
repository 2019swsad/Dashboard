# Database design

要求参考https://sysu-swsad.github.io/swad-guide/08-domain-modeling

数据库设计时主要的目的就是快速搭建和容易扩容和增添属性等,以便以后进行开发时无需过多调整数据库语句  
加上后端采用koa,一个基于express的nodejs服务框架来进行搭建,故采用mongodb是一个比较好也比较符合我们需求的数据库--其轻量,反应速度够快,数据无需处理就能存入表中,是一个很好的面对这种用例不能十分明确的场景,特别是在后期调整某些属性时也能做到无缝适配,免去了更新关系型数据库的痛苦

而对于数据结构的设计,采取了迭代的方法,下面是第一版设计的方案,现在看来有很多遗漏的东西

## User
```JSON
{
    "date":"time",
    "uid":123,
    "username":"",
    "password":"",
    "email":"",
    "phone":"",
    "address":["a","b"],
    "paymentpwd":"",
    "balance":3,
    "credit":10,
    "selftag":["nipple","dick"],
    "systemtag":["asshole"]
}
```

## Task
```JSON
{
    "createDate":"date",
    "expireDate":"date",
    "tid":123,
    "title":"",
    "detail":"",
    "tag":["sucker","jerk"],
    "publisher":"uid",
    "cost":3,
    "receiver":3
}
```

## Order
```JSON
{
    "createDate":"date",
    "oid":123,
    "employer":"uid",
    "employee":"uid",
    "status":1,
    "amount":3
}
```
