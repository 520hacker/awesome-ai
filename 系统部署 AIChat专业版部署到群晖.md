# 系统部署 - AIChat专业版部署到群晖

 购买了nanjiren 的AIChatAdmin 专业版，但是稍微看了下，使用的是sh批处理(https://raw.githubusercontent.com/Nanjiren01/AIChatWeb/main/scripts/setup_pro.sh)的方式安装，habor作为私有docker源，这样方便是方便，也为未来更新版本带来了一些疑虑。这里我特别研究一下群晖是怎么安装的，如果成功，则分享给大家。

根据官方安装脚本，添加habor私有源到群晖; 然后从habor中获取image,然后给几个环境变量就能启动。

```
# Check if /etc/docker/daemon.json exists and modify it
if [ -e /etc/docker/daemon.json ]; then
  echo "daemon.json exists, updating it..."
  jq '.["insecure-registries"] += ["harbor.nanjiren.online:8099"]' /etc/docker/daemon.json > /tmp/daemon.json && mv /tmp/daemon.json /etc/docker/daemon.json
else
  echo "daemon.json does not exist, creating it..."
  echo '{"insecure-registries": ["harbor.nanjiren.online:8099"]}' > /etc/docker/daemon.json
fi
```

貌似不复杂，虽然感觉很厉害哟，但还是让我们开始吧。

## 1、配置docker源

添加私有Habor到群晖，首先要ssh登录

- 在群晖后台-终端机中启用SSH

通过SSH修改/var/packages/Docker/etc/dockerd.json以支持http访问Harbor镜像仓库

我是直接vi 这个文件，直接改成 （群晖这样改，其他平台自己注意啊，改错了我不负责，反正记得备份）

```json
{
  "data-root": "/var/packages/Docker/var/docker",
  "insecure-registries": [
   "harbor.nanjiren.online:8099"
  ],
  "log-driver": "db",
  "registry-mirrors": [],
  "storage-driver": "btrfs"
}
```

然后 :wq ，写入保存退出。

不熟悉VI的人，也有骚操作，就是 

```
cp /var/packages/Docker/etc/dockerd.json /docker/dockerd.json
```

这样 你就可以直接 smb 到对应的路径，用你熟悉的编辑器，比如vscode，去改 dockerd.json ，改完再复制回去

```
sudo cp /docker/dockerd.json /var/packages/Docker/etc/dockerd.json 
```

然后重启docker；参考(https://blog.csdn.net/zlbdmm/article/details/127147114)

然后记得在群晖中增加一个源（注册表=>设置=>新增），虽然最终还是读不了注册表的列表，但是等你pull完之后，就不会报错了。

![image-20230625020351859](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25020351.png)

## 2、通过SSH命令行pull镜像image

很可惜，虽然我按照攻略努力的浪费了很多时间，但是我最终还是没有在群晖控制台的注册表中刷出来相关的镜像（疑惑脸？？）。在没有办法之下，我放弃了群晖界面，尝试进行ssh直接部署。

首先需要登录harbor

```shell
sudo docker login -u zsxq-common -p 'QQ群内获得的密码' http://harbor.nanjiren.online:8099
```

登录成功后，检查一下看看有哪些image是需要先pull下来的，通过 https://raw.githubusercontent.com/Nanjiren01/AIChatWeb/main/docker-compose.yml 可以知道对应的image都是哪些。

如果是社区版，则分别是

- nanjiren01/aichat-admin:latest
- nanjiren01/aichat-web:latest
- nanjiren01/aichat-console:latest
- nanjiren01/aichat-db:latest
- nanjiren01/aichat-redis:latest

如果是专业版，则根据它SH中替换为私有的path，也就是说，你需要先pull这些image

- harbor.nanjiren.online:8099/aichat/aichat-admin:latest
- harbor.nanjiren.online:8099/aichat/aichat-web:latest
- harbor.nanjiren.online:8099/aichat/aichat-console:latest
- harbor.nanjiren.online:8099/aichat/aichat-db:latest
- harbor.nanjiren.online:8099/aichat/aichat-redis:latest

具体就是执行这些命令

```shell
sudo docker pull harbor.nanjiren.online:8099/aichat/aichat-admin:latest
sudo docker pull harbor.nanjiren.online:8099/aichat/aichat-web:latest
sudo docker pull harbor.nanjiren.online:8099/aichat/aichat-console:latest
sudo docker pull harbor.nanjiren.online:8099/aichat/aichat-db:latest
sudo docker pull harbor.nanjiren.online:8099/aichat/aichat-redis:latest
```

太难了， 官方的私有docker真心不快，我花了几个小时才下载下来。

![image-20230625002713091](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25002720.png)

## 3、结合群晖的控制台界面来进行安装

总算集齐5大金刚，开始创建容器。

具体的容器的配置信息来自于 https://raw.githubusercontent.com/Nanjiren01/AIChatWeb/main/docker-compose.yml

```
version: '3'

services:

  admin:
    image: nanjiren01/aichat-admin:latest
    container_name: aichat-admin
    restart: always
    depends_on:
      - db
      - redis
    environment:
      DB_URL: jdbc:mysql://aichat-db:3306/aichat?useSSL=false
      DB_USERNAME: root
      DB_PASSWORD: 123456
      REDIS_HOST: aichat-redis
      REDIS_PORT: 6379
      SUPERADMIN_USERNAME: aichat
      SUPERADMIN_PASSWORD: aichatadmin
      PASSWORD_SALT: any-is-ok
      DEFAULT_TOKENS: 3000
      DEFAULT_CHAT_COUNT: 20
      DEFAULT_ADVANCED_CHAT_COUNT: 2
      DEFAULT_DRAW_COUNT: 0
      MAIL_HOST: smtp.qq.com
      MAIL_PORT: 25
      MAIL_USERNAME: your-email-username
      MAIL_PASSWORD: your-email-password
      TZ: Asia/Shanghai
    ports:
      - "8080:8080"

  web:
    image: nanjiren01/aichat-web:latest
    container_name: aichat-web
    restart: always
    depends_on:
      - admin
    ports:
      - "80:3000"
    environment:
      BASE_URL: http://aichat-admin:8080

  console:
    image: nanjiren01/aichat-console:latest
    container_name: aichat-console
    restart: always
    depends_on:
      - admin
    ports:
      - "8080:80"

  db:
    image: nanjiren01/aichat-db:latest
    container_name: aichat-db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: 123456
      TZ: Asia/Shanghai
    ports:
      - "3310:3306"
    volumes:
      - ./mysql_data:/var/lib/mysql

  redis:
    image: nanjiren01/aichat-redis:latest
    container_name: aichat-redis
    restart: always
  #    ports:
  #      - "6380:6379"
```

按照习惯，我先部署mysql 和 redis ，这2个基本不太会升级。

所有的配置信息都参看以上的问题，环境变量、端口、目录映射这3个是必须要抄上的（也可以根据你的情况进行调整），其他就随意了。

### 3.1  mysql的安装

我猜测已经有mysql的可以忽略此步，不过第一次，我还是看看是不是有什么默认的东西被打到image里了。

- 环境变量全是默认值，123456 你可以改成你心水的密码

![image-20230625003642608](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25003642.png)

- 端口我暂时抄的官方配置指定

![image-20230625003837129](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25003837.png)

- 映射路径 指定自己的文件夹 并 映射到  /var/lib/mysql

![image-20230625003815526](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25003815.png)

 于是就可以连接了，

![image-20230625004545928](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25004545.png)

 果然... mysql 的镜像里放了初始数据库结构，看来我需要做一下迁移才能移除掉这个镜像。

### 3.2 安装redis

看了下，估计是一个普通的匿名redis, 指定一个端口，比如默认的 6379 就好了，如果你局域网内有匿名的redis，那直接可以跳过这一步。

![image-20230625004912946](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25004912.png)

### 3.3 安装核心 aichat-admin

要添加一大堆的环境变量，截图大小就不够用了;  前面装了2个容器，后面也差不多，就是设置环境变量、端口这些重复的事情，就没必要截图了（当心把我密码给截出去了就不好了）。

```
      DB_URL: jdbc:mysql://aichat-db:3306/aichat?useSSL=false
      DB_USERNAME: root
      DB_PASSWORD: 123456
      REDIS_HOST: aichat-redis
      REDIS_PORT: 6379
      SUPERADMIN_USERNAME: aichat
      SUPERADMIN_PASSWORD: aichatadmin
      PASSWORD_SALT: any-is-ok
      DEFAULT_TOKENS: 3000
      DEFAULT_CHAT_COUNT: 20
      DEFAULT_ADVANCED_CHAT_COUNT: 2
      DEFAULT_DRAW_COUNT: 0
      MAIL_HOST: smtp.qq.com
      MAIL_PORT: 25
      MAIL_USERNAME: your-email-username
      MAIL_PASSWORD: your-email-password
      TZ: Asia/Shanghai
```

#### 需要注意的地方：

aichat-db:3306 改成你的 ip + 端口，比如我的 192.168.2.101:3310 

aichat-redis 改成你的 ip , 比如我的 192.168.2.101

用户名和密码自己改，这个不需要解释吧

后面是例子里面是qq邮箱，如何申请qq邮箱的smtp和该怎么配置网上搜索下？或者直接看 https://wx.mail.qq.com/list/readtemplate?name=app_intro.html#/agreement/authorizationCode

然后就是设置端口了，这里非常非常的坑爹，容器名字和端口在 aichat-console 中是写死的，请务必设置成 aichat-admin 和端口 8080:8080 。

### 3.4 安装后台 admin-console

我看环境变量都没有，只有一个端口映射，就意识到这里有一个大坑；果然，在console中写死了连接的地址是 http://aichat-admin:8080 , 这太蠢了，我尝试添加了环境变量 BASE_URL, 然而并没有什么卵用，好吧，老实把 8080 端口给了 aichat-admin ，才连上。

```
  console:
    image: nanjiren01/aichat-console:latest
    container_name: aichat-console
    restart: always
    depends_on:
      - admin
    ports:
      - "8080:80"
```

### 3.5 安装前台 admin-web

还好前台就是很熟悉的 ChatGPTNextWeb了，就是额外添加了一个BASE_URL, 刚才不是被XX了8080端口么，现在好了，就用它了。

```
web:
    image: nanjiren01/aichat-web:latest
    container_name: aichat-web
    restart: always
    depends_on:
      - admin
    ports:
      - "8091:3000"
    environment:
      BASE_URL: http://aichat-admin:8080
```

至此手动安装操作全部完成，各端口占用如下：

| MYSQL   | 3310 |
| ------- | ---- |
| ADMIN   | 8080 |
| REDIS   | 6379 |
| WEB     | 8091 |
| CONSOLE | 8090 |
|         |      |

##  4、调整和更改配置

安装完成之后，我第一时间围观了前台和后台的样子，一个是很熟悉的 ChatGPTNextWeb ， 另外一个是也很熟悉的 VUE element-ui 。虽然很熟悉，但也很陌生，反正很厉害就是了，一点一点做成这样，怎么说也是工程力嘛。

![image-20230625015810434](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25015810.png)

![image-20230625015620717](https://memosfile.qiangtu.com/picgo/assets/2023/06/25202306_25015620.png)



未完待续，也有个没啥东西继续写了。