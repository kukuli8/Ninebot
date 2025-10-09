# 九号出行自动签到脚本

这是一个用于九号出行自动签到的 Node.js 脚本，基于 [wan-kong/ninebot-sign](https://github.com/wan-kong/ninebot-sign) 修改，新增了一些内容。

# 功能特点

- 💻本地运行
- ✅自动签到 (GitHub Actions & 青龙面板)
- 👋单账号/多账号签到
- 🔔Bark 通知推送

# 所需依赖

- axios: "1.6.2"
- moment: "2.29.4"
- dotenv: "16.3.1"


# 环境变量

|变量名|值示例 / 说明|必填|备注|
|---|---|---|---|
|`NINEBOT_DEVICE_ID`|`1234567890`（你的设备唯一标识）|✅|单账号时填写单个 `deviceId`|
|`NINEBOT_AUTHORIZATION`|`Bearer xyz789...`（你的授权令牌）|✅|单账号时填写单个 `authorization`|
|`NINEBOT_ACCOUNTS`|`[{"deviceId":"123","authorization":"Bearer xyz","name":"账号1"},{"deviceId":"456","authorization":"Bearer abc","name":"账号2"}]`|❌|**多账号时使用**，JSON 数组格式（需压缩为单行，无换行符）|
|`BARK_KEY`|`xxxxxxxxxxxxxxxx`（你的 BARK 推送设备密钥）|❌|选填，用于接收签到结果通知（需配合 BARK 应用）|
|`BARK_URL`|`https://api.day.app`（默认值）|❌|选填，自定义 BARK 服务器地址（默认官方地址）|
|`BARK_GROUP`|`九号签到`（推送分组名称）|❌|选填，推送消息的分组标签|
|`BARK_ICON`|`https://example.com/icon.png`（图标 URL）|❌|选填，推送消息显示的图标|
|`BARK_SOUND`|`chime`（铃声名称）|❌|选填，推送消息的铃声（默认 `bell`，支持 `chime`、`alarm` 等）|


# 使用方法

## 抓包方法

脚本签到需要用账号的**设备 ID**（DEVICE_ID）和**授权令牌**（AUTHORIZATION），通过抓包可以获取，这里以 IOS 为例（安卓抓包自行尝试）。

1. 首先，确保手机已安装九号出行 APP，并登录自己的账号。
2. 在 App Store 搜索下载 **Stream** ，根据提示配置CA证书，然后在手机中进行证书信任设置。

![alt text](img/IMG-61ABB21674FC63E60A3E40185AD38F3B.jpeg)

3. 打开 **Stream** 工具，点击开始抓包，然后切换至九号 APP，打开签到页面，之后回到抓包工具，结束抓包。

![alt text](img/IMG-D0C023BE8637EAE98F79A35344F425D9.png)

4. 在抓包历史中找到刚抓包的数据，搜索 **api/user-sign/v2/sign**  字段，点击搜索结果中的第一个 POST 请求数据，打开切换至请求头部分，下面的 Authorization 和 device_id 就是后面需要用的授权令牌和设备 ID。

## 本地运行

1. 本地终端克隆仓库
```bash
git clone https://github.com/waistu/Ninebot.git
```

2. 安装依赖
```bash
npm install
```

3. 设置环境变量（将.env.example 重命名为.env）
```bash
# 九号APP相关（必填）

NINEBOT_DEVICE_ID="你的设备ID"
NINEBOT_AUTHORIZATION="你的授权令牌"
NINEBOT_NAME="你自定义账户名" # 可选

# Bark通知相关（可选，需要则配置）

BARK_KEY=你的Bark密钥
BARK_URL=https://api.day.app # 默认使用 APP 自带服务器

# 以下为Bark可选参数（根据需要添加）
# BARK_GROUP=九号签到通知 # 通知分组
# BARK_ICON=https://xxx.png # 通知图标URL
# BARK_SOUND=bell # 通知铃声
```

4. 运行脚本

```bash
npm start
```

## 青龙面板

1. 添加订阅  https://github.com/waistu/Ninebot.git
2. 安装NodeJs 依赖：axios、moment、dotenv
3. 添加环境变量
4. 修改任务定时

## 使用 GitHub Actions 

1. Fork 本仓库
2. 在仓库的 Settings -> Secrets and variables -> Actions 中添加以下 Secrets：
    - `NINEBOT_DEVICE_ID`: 设备 ID（具体内容自行抓包获取）
    - `NINEBOT_AUTHORIZATION`: 授权令牌（具体内容自行抓包获取）
    - 其他环境变量按需配置，例如 Bark
3. 启用 GitHub Actions
4. 工作流将在每天 08:00 自动运行
5. 也可以手动触发工作流
  
# 免责声明

本项目仅供学习和研究使用，使用本项目产生的任何后果由使用者自行承担。作者不对使用本项目造成的任何损失负责。

本项目不提供任何明示或暗示的保证，包括但不限于对适销性、特定用途适用性和非侵权性的保证。

使用本项目即表示您同意并接受以上免责声明。


