## 快速启动
### 1. 部署mongo

```shell
# 启动mongo容器
docker run -d --net=host --name mongo mongo:4.1.1

# 进入mongo容器后连接数据库
mongo --host 192.168.0.81:27017

# 创建数据库并设置用户名和密码
use blogNode
db.createUser({
  user: "testuser",
  pwd: "password123",
  roles: [
    { role: "readWrite", db: "blogNode" },
    { role: "dbAdmin", db: "blogNode" }
  ]
})
```

### 部署blog-node

```shell
# 修改app.config.js关于mongodb的配置：host、username、password
exports.MONGODB = {
	uri: `mongodb://192.168.0.81:${argv.dbport || '27017'}/blogNode`,
	username: argv.db_username || 'testuser',
	password: argv.db_password || 'password123',
};
# 配置国内镜像代理
yarn config set registry https://registry.npmmirror.com
# 安装依赖并启动服务
yarn install && yarn start
```

### 部署blog-react

```shell
# 根据部署情况修改自身服务端口和后端服务(blog-node)ip+port，如果blog-react和blog-node部署在同一台服务器上，默认不需要修改。后端服务配置在src/setupProxy.js中。
# 配置国内镜像代理
yarn config set registry https://registry.npmmirror.com
# 安装依赖并启动服务
yarn install && yarn start
```

### 部署blog-react-admin

```shell
# 后端服务(blog-node)ip+port配置为config/config.js中的proxy字段，如果blog-react-admin和blog-node部署在同一台服务器上，默认不需要修改。
# 配置国内镜像代理
yarn config set registry https://registry.npmmirror.com
# 用postmain调接口注册
	- method: post
	- url: http://127.0.0.1:3000/register
	- param:
	{
      "name": "BiaoChenXuYing",
      "password": "888888",
      "email": "admin@qq.com",
      "phone": 1380013800,
      "type": 0,
      "introduce":"加班到天明，学习到昏厥!!! 微信公众号：【 BiaoChenXuYing 】，分享 WEB 全栈开发等相关的技术文章，热点资源，全栈程序员的成长之路。"
     }
# 把自己注册的管理员账号的 名字 加在 config/router.config.js 的 authority 里面
# 安装依赖并启动服务
yarn install && yarn start
# 登录：用邮箱和密码登录
```

