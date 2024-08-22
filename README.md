# 微服务划分及功能介绍

### 服务模块划分

1. heoj-backend-common：系统通用模块，比如用户角色权限校验，异常处理，统一返回值，常量，工具类等
2. heoj-backend-file-service：系统文件模块，比如用户头像上传等
3. heoj-backend-gateway：系统网关模块：实现了给前端返回统一接口路由，聚合文档（Knife4j），全局跨域配置，权限校验（JWT Token）等
4. heoj-backend-judge-service：系统判题模块：调用远程代码沙箱接口，实现工厂模式、策略模式、代理模式，验证代码沙箱执行结果是否正确与错误，使用消息队列实现异步处理消息
5. heoj-backend-model：系统实体模块，比如用户实体类、题目实体类，VO、枚举等
6. heoj-backend-question-service：系统题目模块：题目的增删改查、题目提交限流、使用消息队列异步处理消息
7. heoj-backend-service-client：系统内部调用模块，给内部系统提供调用接口
8. heoj-backend-user-service：系统用户模块，管理员对用户的增删改查，用户自己信息查询，修改，头像上传。

## 项目技术栈和特点 ❤️‍🔥

### 后端

1. Spring Boot：简化Spring开发框架
2. Spring MVC：
3. Spring Boot 调试工具和项目处理器
4. Spring AOP 切面编程
5. Spring 事务注解
6. Spring Cloud Alibaba
7. Spring Gateway
8. MyBatis + MyBatis Plus 数据访问（开启分页）
9. MyBatis-Plus 数据库访问结构
10. Redis：分布式存储用户信息
11. Redisson：限流控制
12. JWT Token：用户鉴权
13. RabbitMQ：消息队列
14. Docker 代码沙箱，实现隔离环境运行Java程序
15. Java安全管理器：保护 JVM、Java 安全的机制，实现对资源的操作限制
16. Nacos：服务注册管理中心
17. OpenFeign：微服务模块之间调用

### 前端

1. Vue 3
2. Vue Router: 路由管理
3. Vue-Cli 脚手架
4. Axios: HTTP客户端
5. Bytemd: Markdown 编辑器
6. Monaco Editor: 代码编辑器
7. highlight.js: 语法高亮
8. Moment.js: 日期处理库
9. Arco Design Vue: UI组件库
10. TypeScript: 静态类型系统

### 数据存储

- MySQL 数据库
- 阿里云 OSS 对象存储

### 通用特性

- Spring Session Redis 分布式登录
- 全局请求响应拦截器（记录日志）
- 全局异常处理器
- 自定义错误码
- 封装通用响应类
- Swagger + Knife4j 接口文档
- 自定义权限注解 + 全局校验
- 全局跨域处理
- 长整数丢失精度解决
- 多环境配置
- IDEA插件 MyBatisX ： 根据数据库表自动生成
- Hutool工具库 、Apache Common Utils、Gson 解析库、Lombok 注解

### 单元测试

- JUnit5 单元测试、业务功能单元测试

### 设计模式

- 静态工厂模式
- 代理模式
- 策略模式

### 远程开发

- VMware Workstation虚拟机
- Ubuntu Linux 18
- Docker环境
- 使用JetBrains Client连接


### 依赖服务
 注册中心：Nacos
 微服务网关（heoj-backend-gateway ，8101端口 ）：Gateway 聚合所有的接口，统一接受处理前端的请求

公共模块：
 common 公共模块（heoj-backend-common）：全局异常处理器、请求响应封装类、公用的工具类等
 model 模型模块（heoj-backend-model）：很多服务公用的实体类
 公用接口模块（heoj-backend-service-client）：只存放接口，不存放实现（多个服务之间要共享）

代码沙箱（shier-code-sandbox，8081 端口，只做判题，得到判题结果）：

代码沙箱服务本身就是独立的，不用纳入 Spring Cloud 的管理

1. 执行提交的程序
2. 返回执行程序信息

### 业务功能：

1. 用户服务（heoj-backend-user-service：8105 端口）：
   a. 注册（后端已实现）
   b. 登录（后端已实现，前端已实现）
   c. 用户管理
2. 题目服务（heoj-backend-question-service：8106 端口）
   a. 创建题目（管理员）
   b. 删除题目（管理员）
   c. 修改题目（管理员）
   d. 搜索题目（用户）
   e. 在线做题（题目详情页）
   f. 题目提交
3. 判题服务（heoj-backend-judge-service，8107 端口，较重的操作）
   a. 执行判题逻辑
   b. 错误处理（内存溢出、安全性、超时）
   c. 自主实现 代码沙箱（安全沙箱）
   d. 开放接口（提供一个独立的新服务）
4. 文件上传（heoj-backend-file-service，8108 端口）
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
