# NDU(Notify Docker Update)

![GitHub repo size](https://img.shields.io/github/repo-size/htnanako/ndu)
![Docker Image Size (tag)](https://img.shields.io/docker/image-size/htnanako/ndu/latest)
![Docker Pulls](https://img.shields.io/docker/pulls/htnanako/ndu)
[![GitHub Repo stars](https://img.shields.io/github/stars/htnanako/ndu?style=social)](https://github.com/htnanako/ndu/stargazers)

# About

### 本项目用于监测Dockerhub的镜像更新，并推送通知提醒。
### 支持多通知渠道(bark,telegram,企业微信)
### 支持多镜像多tag同时监测

# Start

### Docker部署

- Docker Cli
```shell
docker run -itd --name=ndu -v /path/to/ndu:/data -p 5050:5050 -e HUB_PROXY=http://127.0.0.1:6152 htnanako/ndu:latest
```

- Docker Compose
```yaml
version: "3"
services:
  ndu:
    image: htnanako/ndu:latest
    container_name: ndu
    network_mode: bridge
    ports:
      - 5050:5050
    volumes:
      - ./:/data
    environment:
      HUB_PROXY: http://127.0.0.1:6152 # 访问DOCKERHUB使用的代理，支持http,sock5
```

### Configuration

> 容器启动后，先关闭容器。
> 
> 在映射路径下找到conf/base_config.yml文件
> 
> 自动生成的配置文件如下所示

```yaml
ndu:
  interval: 60 # 监测任务的间隔时间，单位为秒
  registry:
    username: '' # 暂时无用，可不填
    password: '' # 暂时无用，可不填
  notify: bark,telegram,qywx # 通知渠道，可选bark,telegram,qywx，多个渠道用逗号分隔
  github_token: '' # Github Personal access tokens，用于规避Github API限流，获取方式看下面，可不填
notify:
  public:
    dockerhub_notify_img: '' # Dockerhub更新通知图片，有需要可自定义，不填使用系统默认图
    github_notify_img: '' # GitHub更新通知图片，有需要可自定义，不填使用系统默认图
  qywx:
    qywx_base_url: https://qyapi.weixin.qq.com # 企业微信API地址，有需要可自定义
    corpid: '' # 企业微信corpid
    corpsecret: '' # 企业微信corpsecret
    agentid: '' # 企业微信agentid
    touser: '@all' # 企业微信通知对象，@all为所有人，也可指定成员，暂时不支持多个
  bark:
    bark_url: '' # bark接口地址及Key
    bark_sound: chime # bark通知铃声
    bark_group: NDU # bark通知分组
    bark_icon: '' # bark通知图标
  telegram:
    tg_base_url: https://api.telegram.org/bot # telegram的api服务地址，默认是官方，可以改为自建
    tgbot_token: '' # telegram bot的Token，BotFather获取到的
    tg_chat_id: '' # 接收消息的chat_id，可为频道、群组或个人
    proxy: '' # 留空则不使用代理。支持通过HTTP代理、SOCKS代理发送消息。示范：http://localhost:8030 或 socks5://user:pass@host:port
images:
- htnanako/ndu:latest # 填写需要监测的镜像名:tag，如不加tag默认为latest，多个镜像按相同格式一行一个
github_repos:
- htnanako/ndu # 填写需要检测的Github仓库名，多个仓库按相同格式一行一个
```


### Github Personal access tokens获取
- 打开这个页面[GithubToken](https://github.com/settings/tokens)
- 点击Generate new token，选择`Generate new token(classic)`
- 名称(Note)可自己命名，`Expiration`是Token有效期，可选择`No Expiration`，即为永久可用。
- 勾选`repo`权限。
- 点击`Generate token`，复制生成的token填入配置文件中的github_token字段。

### 测试程序
```shell
curl --request GET \
  --url http://127.0.0.1:5050/api/test # 根据实际情况填写IP:PORT
```

或者浏览器直接访问`http://127.0.0.1:5050/api/test` # 根据实际情况填写IP:PORT

正常的响应：
```json
{
  "success": true,
  "errorCode": 0,
  "message": "测试成功。"
}
```
同时设定好的通知渠道会收到测试通知

### 通知模板

程序已内置通知模板，如有需要可自行修改。
使用Jinja2模板语法，具体语法可参考[Jinja2](https://docs.jinkan.org/docs/jinja2/)。
#### 内置变量
| 变量名 |           描述            |
|:---:|:-----------------------:|
| image_name |    镜像名，仅docker更新推送可用    |
| image_tag |   镜像tag，仅docker更新推送可用   |
| image_update_time |  镜像更新时间，仅docker更新推送可用   |
| image_platform |   镜像平台，仅docker更新推送可用    |
| image_platform | 镜像SHA256值，仅docker更新推送可用 |
|  |  |
| repo_name |   仓库名，仅github更新推送可用    |
| release_version |  发布版本，仅github更新推送可用   |
| release_time |  发布时间，仅github更新推送可用   |
| release_log |  发布日志，仅github更新推送可用   |


## TODO
- [x] 增加测试程序接口
- [x] 增加Github Repo Release监控功能
- [x] 支持jinja2模板语法自定义通知内容


## 支持一下

你可以请我喝一杯咖啡，以表示对这个项目的支持😉

<img src="https://ossapi.ainnk.vip/public/images/MovieBot_Plugins/donate/nanako_qrcode.jpg" width="300" />
