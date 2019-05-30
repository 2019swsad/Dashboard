# 功能模型
## 注册登录
- 注册：
  - 注册时，前端循环检查内容输入格式是否正确；
  - 用户提交注册信息后，后端检查用户名是否已存在：
    - 若不存在，则在数据库添加新用户信息，并返回“注册成功”信息；
    - 若已存在，则返回“用户已存在”信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_register.jpg)

- 登录
  - 用户在前端输入登录信息；
  - 前端向后端提交信息，后端对信息进行验证；
  - 循环该过程直至信息验证成功；
  - 前端跳转至主页面；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_login.jpg)

## 一般业务
- 修改个人信息：
  - 用户向前端提交需要修改的信息；
  - 后端完成信息的更新；
  - 前端更新页面，以显示新的信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_modifyInfo.jpg)

- 发布任务：
  - 用户在前端输入任务发布信息；
  - 前端向后端提交信息；
  - 后端计算发布者的余额是否充足：
    - a. 若余额不足：
    - a1. 返回“余额不足”信息；
    - b. 若余额充足：
    - b1. 冻结用户用于酬劳的余额；
    - b2. 在后端添加任务信息；
    - b3. 返回“发布成功”信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_releaseTask 2.0.jpg)

- 报名
  - 用户向前端发送报名申请；
  - 前端向后端转发报名申请；
  - 后端检查任务的当前报名人数：
    - 若报名人数未超额，则将用户加入报名列表中；
    - 若报名人数超额，则将用户加入候补列表中；
    - 返回报名情况信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_applyTask 2.0.jpg)

- 查找任务：
  - 用户向前端提交“查询任务”请求，并可选排序、分类、关键词检索功能；
  - 前端向后端请求符合关键词和分类的任务列表；
  - 前端对返回的任务列表进行排序；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_getTaskList 2.0.jpg)

- 获取任务详情
  - 用户向前端发送“获取任务详情”的请求；
  - 前端将被请求任务的tid及用户的uid发送给后端；
  - 后端向前端返回相应任务详情，并返回用户与任务的关系；
  - 前端根据用户与任务的关系，提供不同的“任务详情”页面（不同角色有不同操作权限）
![Usecase Diagram](/assets/System Sequence Diagram/SSD_getTaskDetail 2.0.jpg)

- 查看消息
  - 用户向前端发送“查看消息”请求；
  - 前端向后端转发请求；
  - 后端返回相应用户的所有通知消息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_getMessage.jpg)

## 任务管理
- 删除任务：
  - 发布者向前端发送删除任务请求；
  - 前端将请求转发至后端；
  - 后端检查用户删除任务的频次：
    - a. 若频次没有超过阈值：
    - a1. 解冻发布者的酬劳余额；
    - a2. 删除任务信息；
    - b. 若频次超过阈值：
    - b1. 降低发布者的信誉值；
    - b2. 解冻发布者的酬劳余额；
    - b3. 删除任务信息；
    - b4. 返回“降低信誉值”信息；
    - 返回“删除成功”信息;
    - 后端通知参与者“删除任务”信息
![Usecase Diagram](/assets/System Sequence Diagram/SSD_deleteTask 3.0.jpg)

- 退出任务：
  - 参与者向前端发送退出任务请求；
  - 前端将请求转发至后端；
  - 后端检查任务状态：
    - a. 若任务处于“报名状态”；
    - a1. 将参与者从报名名单中移除；
    - b. 若任务处于“进行状态”；
    - b1. 降低参与者的信誉值；
    - b2. 将参与者从报名名单中移除；
    - b3. 返回“降低信誉值”信息；
    - 返回“退出成功”信息
    - 后端通知发布者“退出任务”信息
![Usecase Diagram](/assets/System Sequence Diagram/SSD_quitTask 2.0.jpg)

- 查看报名情况：
  - 发布者向前端发送“查看报名情况”请求；
  - 前端向后端转发请求；
  - 后端返回报名列表与候补列表；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_getApplicantList.jpg)
 
- 取消报名资格：
  - 发布者向前端发送取消某位报名者的报名资格请求；
  - 前端向后端提交信息；
  - 后端将相应报名者从报名列表中移除；
  - 后端向前端返回“取消成功”信息；
  - 前端向后端重新请求报名列表；
  - 后端返回报名列表，前端更新页面；
  - 返回“取消成功”信息；
  - 后端通知参与者“取消报名资格”信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_removeApplicant 2.0.jpg)

- 选择候补人员：
  - 发布者向前端发送选择特定候补人员的请求；
  - 前端向后端转发请求；
  - 后端检查报名者数量是否超额：
    - a. 若未超额：
    - a1. 将特定人员从候补名单移至报名名单；
    - a2. 返回成功信息；
    - a3. 前端向后端请求报名列表；
    - a4. 后端返回结果，前端更新页面
    - a5. 后端通知候补人员转正信息；
    - b. 若超额：
    - b1. 返回操作失败信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_qualifySubstitute 2.0.jpg)

- 开始任务
  - 发布者向前端发送开始任务的请求；
  - 前端向后端转发请求；
  - 后端检查任务报名人数：
    - a. 若报名人数不为0：
    - a1. 后端将任务设置为开始状态；
    - a2. 后端生成任务完成码；
    - a3. 后端返回开始信息与完成码；
    - a4. 后端通知参与者任务已开始；
    - b. 若报名人数为0：
    - b1. 返回“人数不足”信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_beginTask 2.0.jpg)

- 完成任务
  - 参与者在前端输入完成码，申请完成任务；
  - 前端向后端转发请求；
  - 后端验证完成码：
    - a. 若完成码正确：
    - a1. 将参与者状态改为“完成”；
    - a2. 从冻结的酬劳中向参与者发起支付；
    - a3. 返回完成与支付信息；
    - a4. 后端向发布者返回完成与支付信息；
    - b. 若完成码错误：
    - b1. 返回“完成码错误”信息；
![Usecase Diagram](/assets/System Sequence Diagram/SSD_finishTask 2.0.jpg)
