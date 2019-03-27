# Database design
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