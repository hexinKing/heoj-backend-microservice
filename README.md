# 微服务划分及功能介绍

### 依赖服务
 注册中心：Nacos
 微服务网关（shieroj-backend-gateway ，8101端口 ）：Gateway 聚合所有的接口，统一接受处理前端的请求

公共模块：
 common 公共模块（shieroj-backend-common）：全局异常处理器、请求响应封装类、公用的工具类等
 model 模型模块（shieroj-backend-model）：很多服务公用的实体类
 公用接口模块（shieroj-backend-service-client）：只存放接口，不存放实现（多个服务之间要共享）

代码沙箱（shier-code-sandbox，8081 端口，只做判题，得到判题结果）：

代码沙箱服务本身就是独立的，不用纳入 Spring Cloud 的管理

1. 执行提交的程序
2. 返回执行程序信息

### 业务功能：

1. 用户服务（shieroj-backend-user-service：8105 端口）：
   a. 注册（后端已实现）
   b. 登录（后端已实现，前端已实现）
   c. 用户管理
2. 题目服务（shieroj-backend-question-service：8106 端口）
   a. 创建题目（管理员）
   b. 删除题目（管理员）
   c. 修改题目（管理员）
   d. 搜索题目（用户）
   e. 在线做题（题目详情页）
   f. 题目提交
3. 判题服务（shieroj-backend-judge-service，8107 端口，较重的操作）
   a. 执行判题逻辑
   b. 错误处理（内存溢出、安全性、超时）
   c. 自主实现 代码沙箱（安全沙箱）
   d. 开放接口（提供一个独立的新服务）
4. 文件上传（shieroj-backend-file-service，8108 端口）
   a. 头像文件上传

### 路由划分

1. 用户服务：
    /api/user
    /api/user/inner（内部调用，网关层面要做限制）
2. 题目服务：
    /api/question（也包括题目提交信息）
    /api/question/inner（内部调用，网关层面要做限制）
3. 判题服务：
    /api/judge
    /api/judge/inner（内部调用，网关层面要做限制）
