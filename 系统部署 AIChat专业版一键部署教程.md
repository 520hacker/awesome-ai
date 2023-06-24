# 系统部署 | AIChat专业版一键部署教程

AIChat团队 [夜光之城](javascript:void(0);) *2023-06-24 00:58* *发表于重庆*

 

本文将详细介绍AIChat专业版Docker Compose一键部署的具体流程和步骤。部署步骤后续可能会有变化，请及时关注最新的部署方案。

## 1 执行部署脚本

在命令行中输入：

```
bash <(curl -s https://raw.githubusercontent.com/Nanjiren01/AIChatWeb/main/scripts/setup_pro.sh)
```

## 2 获取专业版授权

在执行步骤1中的部署脚本中，会提示你输入AIChat专业版的授权账户用户名和密码（请在加入AIChat的知识星球专属QQ群后获取），如下图所示：

```
Please input AIChat Pro authorized account username:
Username: AIChat专业版的授权账户用户名
Please input AIChat Pro authorized account password:
Password: AIChat专业版的授权账户密码
```

请在 `Username:` 后填写AIChat专业版的授权账户用户名，在 `Password:` 后填写AIChat专业版的授权账户密码，若登录成功，则会提示 `Login Succeeded` 。

## 3 设置管理前台超管账户

在执行步骤1中的部署脚本中，会提示你设置管理前台的超级管理员账户及密码，请在 `Username:` 后填写你要设置的超管账户用户名，如下所示：

```
Please input the super admin username. 
Only letters and numbers are supported, the length should between 6 and 20, and they cannot start with a number.
Username: 你要设置的超管账户用户名   
```

如果超级管理员账户名有效，会提示 `Super Admin Username is valid.` ，并且会接着提示你输入超级管理员账户的密码，请在 `Password:`后填写你要设置的超管账户密码，如下所示：

```
Please input the super admin password. 
Only letters and numbers are supported, and the length should between 6 and 20. 
You can change it on the web page after the Application running
Password: 你要设置的超管账户密码
```

如果超级管理员账户密码有效，会提示 `Super Admin Password is valid.`

## 4 完成AIChat专业版部署

当命令行窗口出现以下信息时，则表示部署成功：

```
[+] Running 6/6
 ✔ Network root_default      Created                                                                                                                                                                                                                0.0s 
 ✔ Container aichat-db       Started                                                                                                                                                                                                                2.8s 
 ✔ Container aichat-redis    Started                                                                                                                                                                                                                2.9s 
 ✔ Container aichat-admin    Started                                                                                                                                                                                                                1.1s 
 ✔ Container aichat-web      Started                                                                                                                                                                                                                1.8s 
 ✔ Container aichat-console  Started                                                                                                                                                                                                                1.8s 
```

## 5 访问AIChat专业版

稍等几秒钟应用初始化，即可打开 `http://IP` 访问用户前台页面，打开 `http://IP:8080` 访问管理前台页面。