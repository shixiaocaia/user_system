## 用户管理系统

原项目来源于：[Percygu](https://github.com/Percygu/user_system)

- 前端：用户登录，用户注册，昵称修改，上传头像，用户注销
- 后端：用户注册，登录，登出，上传用户头像，用户注销

在原项目基础上实现了前后端的用户头像上传和用户注销。

## 快速开始
### mysql
1. 用docker启动一个mysql容器
```shell
docker run --name camps_mysql -e MYSQL_ROOT_PASSWORD=123456 -d -e MYSQL_DATABASE=camps_user -p 8086:3306 mysql:8.0
```
2. 进入mysql容器
```shell
docker run -it --network=host mysql:8.0 mysql -h 0.0.0.0 camps_user -P 8086 -u root -p
```
3. 创建表
```sql
use camps_user;
CREATE TABLE IF NOT EXISTS t_user(
   `id` INT NOT NULL AUTO_INCREMENT,
   `name` VARCHAR(100) NOT NULL,
   `age` int NOT NULL,
   `gender` VARCHAR(30) NOT NULL,
   `password` varchar(255) NOT NULL DEFAULT '',
   `nickname` varchar(100) NOT NULL DEFAULT '',
   `head_url` varchar(1024) NOT NULL DEFAULT '',
   `create_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
   `creator` VARCHAR(100) NOT NULL DEFAULT '',
   `modify_time` timestamp NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '最后一次修改时间',
   `modifier` VARCHAR(100) NOT NULL DEFAULT '',
   PRIMARY KEY ( id )
);
```
### redis
1. 用docker启动一个redis容器
```shell
docker run --name camps_redis -d -p 8089:6379 redis:6.2-rc2
```
2. 进入redis容器
```shell
docker ps
docker exec -it <CONTAINER ID> redis-cli -h 0.0.0.0 -p 6379
```
### 启动项目
1. 启动后端
```shell
cd cmd
go run main.go
```
2. 打开浏览器输入：`http://localhost:8080/ping`, 返回相应信息。
3. 用户注册：`http://localhost:8080/static/register.html`
4. 用户登录：`http://localhost:8080/static/login.html`


## 学习心得

- 中间的session作用是什么
  - 用于保存用户登录状态，如果没有session，每次请求都需要重新登录
  - 避免使用jwt，因为jwt是无状态的，不安全
- 日志文件使用的是log
  - 可以换用 zap, logrus自定义日志格式
- 使用热启动
  - 使用gin的热启动，可以在修改代码后，不需要重新编译，直接生效
- 数据库存储密码的时候，应该加密，不能明文存储
  - 使用bcrypt加密
  - 使用bcrypt.CompareHashAndPassword()验证密码

