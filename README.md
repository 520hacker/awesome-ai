# 平民开源AI转发应用总结 


目前平民AI分几个流派

- 直接转发API, 相当于代理
- 为了自己用，只能ChatGPT
- 为了卖，只能ChatGPT
- 为了卖，可以ChatGPT,可以ChatGPT绘画
- 为了卖，可以ChatGPT,可以ChatGPT绘画，可以Midjourney绘画
- 可以ChatGPT,可以上传文件进行向量查询
- 为了卖，可以ChatGPT, 可以微信
- 为了卖，可以ChatGPT,可以Midjourney绘画, 可以微信
- 为了卖，可以ChatGPT,可以Midjourney绘画, 可以微信，可以QQ

于是我制作了以下表格, 个人信息源总有遗漏，需要补充和更新的可以联系我

请在 http://memos.qiangtu.com/m/1081 留言或者直接 https://t.me/Odinluo 

### API  

| 项目                                                       | api  | MEMBER | pay  |
| ---------------------------------------------------------- | ---- | ------ | ---- |
| API2D                                                      | Y    | N      | N    |
| [songquanpeng/one-api](https://github.com/songquanpeng/one-api)               | Y    | Y      | Y    |
| [mirrors2/opencatd-open](https://github.com/mirrors2/opencatd-open) | Y    | N      | N    |

### 微信&QQ

| 项目                                                         | CHATBox | DALL-E | MJ   | Telegram | QQ   | wechat | VDB  | MEMBER | pay  |
| ------------------------------------------------------------ | ------- | ------ | ---- | -------- | ---- | ------ | ---- | ------ | ---- |
| [zhayujie/chatgpt-on-wechat](https://github.com/zhayujie/chatgpt-on-wechat)     | N       | Y      | N    | N        | N    | Y      | Y    | N      | N    |
| [1130600015/MidJourney_QQ](https://github.com/1130600015/MidJourney_QQ) | N       | N      | Y    | N        | Y    | N      | N    | N      | N    |
| [RockChinQ/QChatGPT](https://github.com/RockChinQ/QChatGPT)            | N       | Y      | N    | N        | Y    | N      | N    | N      | N    |
| [lss233/chatgpt-mirai-qq-bot](https://github.com/lss233/chatgpt-mirai-qq-bot) | Y       | Y      | N    | Y        | Y    | Y      | N    | N      | N    |
| [LlmKira/Openaibot](https://github.com/LlmKira/Openaibot)            | N       | N      | N    | Y        | Y    | N      | N    | N      | N    |
| [fuergaosi233/wechat-chatgpt](https://github.com/fuergaosi233/wechat-chatgpt) | N       | Y      | N    | N        | N    | Y      | N    | N      | N    |

### Web 

``` 
评分标准
DALL-E: DALL-E 绘画支持
MJ: midjourney support
MP: 微信公众号对话支持
VDB: 向量数据库支持，上传文件对话支持
Member: 会员系统，收费系统
Beauty: UI美观度
Status: 最近更新时间 2023-06-30 ， 距离当前时间每多1天多扣0.1
```

| 项目                                                         | DALL-E | MJ   | MP   | VDB  | MEMBER | BEAUTY | STATUS | SCORE |
| ------------------------------------------------------------ | ------ | ---- | ---- | ---- | ------ | ------ | ------ | ----- |
| [apeto2/gpt-commercial](https://github.com/apeto2/gpt-commercial) | N      | Y    | Y    | N    | Y      | 3      | 0      | 6     |
| [langgenius/dify](https://github.com/langgenius/dify)        | N      | N    | N    | Y    | Y      | 4      | 0      | 6     |
| [Nanjiren01/AIChatWeb](https://github.com/Nanjiren01/AIChatWeb) | N      | N    | N    | N    | Y      | 5      | -0.2   | 5.8   |
| [Licoy/ChatGPT-Midjourney](https://github.com/Licoy/ChatGPT-Midjourney) | N      | Y    | N    | N    | N      | 5      | -0.3   | 5.7   |
| [AprNEA/ChatGPT-Admin-Web](https://github.com/AprilNEA/ChatGPT-Admin-Web) | N      | N    | N    | N    | Y      | 5      | -0.9   | 5.1   |
| [Yidadaa/ChatGPTNextWeb](https://github.com/Yidadaa/ChatGPT-Next-Web) | N      | N    | N    | N    | N      | 5      | 0      | 5     |
| [longyanjiang/chatgpt-nine-ai](https://github.com/longyanjiang/chatgpt-nine-ai) | Y      | Y    | N    | N    | Y      | 3      | -1.5   | 4.5   |
| [moeakwak/chatgpt-web-share](https://github.com/moeakwak/chatgpt-web-share) | N      | N    | N    | N    | Y      | 4      | -0.9   | 4.1   |
| [AndersonBY/vector-vein](https://github.com/AndersonBY/vector-vein) | N      | N    | N    | Y    | N      | 3      | -0.3   | 3.7   |
| [yangjiakai/lux-admin-vuetify3](https://github.com/yangjiakai/lux-admin-vuetify3) | N      | N    | N    | N    | Y      | 4      | -2.4   | 3.6   |
| [79E/ChatGpt-Web](https://github.com/79E/ChatGpt-Web)        | Y      | N    | N    | N    | Y      | 2      | -0.4   | 3.6   |
| [putyy/chatgpt](https://github.com/putyy/chatgpt)            | N      | N    | N    | N    | Y      | 3      | -0.5   | 3.5   |
| [EniasCailliau/GirlfriendGPT](https://github.com/EniasCailliau/GirlfriendGPT) | N      | N    | N    | N    | N      | 4      | -0.7   | 3.3   |
| [vastxie/ChatGpt-Web](https://github.com/vastxie/ChatGpt-Web) | Y      | N    | N    | N    | N      | 3      | -1     | 3     |
| [Chanzhaoyu/chatgpt-web](https://github.com/Chanzhaoyu/chatgpt-web) | N      | N    | N    | N    | N      | 3      | -0.4   | 2.6   |
| [pengzhile/pandora](https://github.com/pengzhile/pandora)    | N      | N    | N    | N    | N      | 3      | -0.4   | 2.6   |
| [xyhelper/chatgpt-mirror-server](https://github.com/xyhelper/chatgpt-mirror-server) | N      | N    | N    | N    | N      | 3      | -0.5   | 2.5   |
| [adams549659584/go-proxy-bingai](https://github.com/adams549659584/go-proxy-bingai) | Y      | N    | N    | N    | N      | 4      | -2.8   | 2.2   |
| [gptlink/gptlink](https://github.com/gptlink/gptlink)        | N      | N    | N    | N    | Y      | 2      | -1     | 2     |
| [qnmlgbd250/appset](https://github.com/qnmlgbd250/appset)    | N      | N    | N    | N    | N      | 2      | 0      | 2     |


### 重要组件

| 项目                                                         | api  | DALL-E | MJ   | wan load |
| ------------------------------------------------------------ | ---- | ------ | ---- | -------- |
| [novicezk/midjourney-proxy](https://github.com/novicezk/midjourney-proxy) | Y    | N      | Y    | N        |
| [mouxangithub/midjourney](mouxangithub/midjourney)           | Y    | N      | Y    | N        |
| [goldfishh/chatgpt-tool-hub](goldfishh/chatgpt-tool-hub)     | Y    | N      | N    | Y        |

### 客户端

| 项目                              | MAC  | WIN  | IOS  | ANDRIOD | 开源 |
| --------------------------------- | ---- | ---- | ---- | ------- | ---- |
| [AMA](https://bytemyth.com/zh-CN) | Y    | Y    | Y    | Y       | ？   |

