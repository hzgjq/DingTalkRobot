# DingTalkPush
利用钉钉自定义机器人进行消息推送的bash脚本

## 脚本使用方法

### 步骤一：创建自定义机器人
[钉钉自定义机器官方说明文档](https://developers.dingtalk.com/document/app/custom-robot-access)
1. 打开机器人管理页面。以PC端为例，打开PC端钉钉，点击头像，选择机器人管理。
![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4099076061/p131222.png)

2. 在机器人管理页面选择自定义机器人，输入机器人名字并选择要发送消息的群，同时可以为机器人设置机器人头像。
![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4824199951/p131223.png)

3. 完成必要的安全设置，勾选**我已阅读并同意《自定义机器人服务及免责条款》**，然后单击**完成**。

目前有3种安全设置方式，请根据需要选择一种：

- 自定义关键词：最多可以设置10个关键词，消息中至少包含其中1个关键词才可以发送成功。

例如添加了一个自定义关键词：监控报警，则这个机器人所发送的消息，必须包含监控报警这个词，才能发送成功。

- 加签：发送消息时需要将时间戳和指定的密钥使用HmacSHA256算法计算签名后拼接到URL中。

- IP地址（段）：设定后，只有来自IP地址范围内的请求才会被正常处理。支持两种设置方式：IP地址和IP地址段，暂不支持IPv6地址白名单，格式如下。

|  格式  | 说明  |
|  ----  | ----  |
|  1.1.1.1  | 开发者的出口公网IP地址（非局域网地址）  |
| 1.1.1.0/24  | 用CIDR表示的一个网段 |
4. 完成安全设置后，复制出机器人的Webhook地址，可用于向这个群发送消息，格式如下：

https://oapi.dingtalk.com/robot/send?access_token=XXXXXX

**注意** 请保管好此Webhook 地址，不要公布在外部网站上，泄露后有安全风险。

## 步骤二：修改配置文件
修改配置文件`dingtalkpush.conf`中和自定义机器人有关的变量，

`APIURL='https://oapi.dingtalk.com/robot/send'` *机器人API地址，无需修改*

`Token='XXXXXXXXXXXXXXXXXX'`  *机器人Token，Webhook地址access_token=后面的字符串，共64位，注意大小写*

`Secret='SEC5fff35d403cbc3fc8a65xxxxxxxxxxxxxxxxxxxx'` *加签密钥，如不使用则留空*

**其余变量作用详见注释**

## 步骤三：测试程序
在Linux上执行程序`dingtalkpush -e`

此时钉钉上应该已经收到了内置的测试消息

可以通过 `dingtalkpush --help` 查看更多用法


## 消息类型及数据格式
脚本支持发送text类型、link类型、markdown类型、整体跳转ActionCard类型、独立跳转ActionCard类型、FeedCard类型。

不同类型的消息内容均为符合json格式的字符串，具体格式请参考[钉钉自定义机器官方说明文档](https://developers.dingtalk.com/document/app/custom-robot-access)。
