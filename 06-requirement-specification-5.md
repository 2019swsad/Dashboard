# 功能模型
- 注册：
  - 注册时，前端循环检查内容输入格式是否正确；
  - 用户提交注册信息后，后端检查用户名是否已存在：
    - 若不存在，则在数据库添加新用户信息，并返回“注册成功”信息；
    - 若已存在，则返回“用户已存在”信息；
![Usecase Diagram](/assets/SSD_register.jpg)

- 登录
  - 用户在前端输入登录信息；
  - 前端向后端提交信息，后端对信息进行验证；
  - 循环该过程直至信息验证成功；
  - 前端跳转至主页面；
![Usecase Diagram](/assets/SSD_login.jpg)

- 修改个人信息：
  - 用户向前端提交需要修改的信息；
  - 后端完成信息的更新；
  - 前端更新页面，以显示新的信息；
![Usecase Diagram](/assets/SSD_modifyInfo.jpg)

- 发布任务：
  - 用户在前端输入任务发布信息；
  - 前端向后端提交信息；
  - 后端计算发布者的余额是否充足：
    - a1. 若余额不足，返回“余额不足”信息；
    - b1. 若余额充足，冻结用户用于酬劳的余额；
    - b2. 在后端添加任务信息；
    - b3. 返回“发布成功”信息；
![Usecase Diagram](/assets/SSD_releaseTask.jpg)

- 查找任务：
  - 用户向前端提交“查询任务”请求，并可选排序、分类、关键词检索功能；
  - 前端向后端请求符合关键词和分类的任务列表；
  - 前端对返回的任务列表进行排序；
![Usecase Diagram](/assets/SSD_getTaskList 2.0.jpg)

- 获取任务详情
  - 用户向前端发送“获取任务详情”的请求；
  - 前端将被请求任务的tid及用户的uid发送给后端；
  - 后端向前端返回相应任务详情，并返回用户与任务的关系；
  - 前端根据用户与任务的关系，提供不同的“任务详情”页面（不同角色有不同操作权限）
![Usecase Diagram](/assets/SSD_getTaskDetail 2.0.jpg)

- 报名
![Usecase Diagram](/assets/SSD_applyTask.jpg)
- 任务管理
  - 删除任务：在发布者删除任务时，后端会检测用户删除任务的频率。如果用户频繁删除任务，则会扣除用户的信誉值并告知用户。在任务被从数据库中删除前，后端会对已完成任务的参与者发起结算。结算完毕后会解冻发起者的剩余款项。任务被删除后，会告知发布者与参与者。
  ![Usecase Diagram](/assets/SSD_deleteTask.jpg)
  - 退出任务：在参与者退出任务时，后端会检测当前任务的状态。如果任务仍处于报名状态，参与者可直接退出任务；如果任务处于进行状态，后端会对参与者的信誉值进行扣除。
  ![Usecase Diagram](/assets/SSD_quitTask.jpg)
  - 取消报名资格：在任务处于报名状态时，发布者可以取消特定报名者的报名资格。资格被取消后，报名者会收到相应通知。
  ![Usecase Diagram](/assets/SSD_removeApplicant.jpg)
  - 选择替补人员：在任务处于报名状态时，发布者可以在取消特定报名者资格后，选择替补人员参与任务。被选择的替补人员会收到相应通知。
  ![Usecase Diagram](/assets/SSD_qualifySubstitute.jpg)
