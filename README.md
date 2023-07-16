## 用户管理系统

原项目来源于：[Percygu](https://github.com/Percygu/user_system)

### 项目介绍

- 前端：用户登录，用户注册，昵称修改，上传头像
- 后端：用户注册，登录，登出，上传用户头像，用户注销

### 快速开始
- 启动mysql, redis
```shell
# 启动mysql
docker run -it --network=host mysql:8.0 mysql -h 0.0.0.0 camps_user -P 8086 -u root -p
# 启动redis
docker ps
docker exec -it <CONTAINER ID> redis-cli -h 0.0.0.0 -p 6379
```

### Todo

- [x] 前端用户注销
- [x] 后端用户注销
- [x] 后端上传用户头像
  - 保存文件
  - 更新redis, mysql
  - 用户登陆时，从redis中或者mysql中获取用户信息HeadUrl

### 学习心得

- 中间的session作用是什么 ？
  - 用于保存用户登录状态，如果没有session，每次请求都需要重新登录
  - 可以换用jwt，不需要保存session
- 日志文件使用的是log
  - 可以换用 zap, logrus自定义日志格式
- 使用热启动
  - 使用gin的热启动，可以在修改代码后，不需要重新编译，直接生效
- 数据库存储密码的时候，应该加密，不能明文存储
  - 使用bcrypt加密
  - 使用bcrypt.CompareHashAndPassword()验证密码

