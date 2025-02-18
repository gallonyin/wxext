# e小天

pc微信小助手,软件本地运行，不联网，安全可靠

本机进程管理工具,方便管理本机微信

旨在提高生产生活效率，禁止骚扰营销

[:memo: 编辑本文档](https://github.com/wxext/wxext/blob/master/docs/home/README.md)

# 开发准备

+ [下载服务端](https://pan.wyfxw.cn/plainwizard/Setup_wxext.msi "下载e小天")

+ [PC微信安装](https://pc.weixin.qq.com/ "微信PC版")自适应匹配最新版PC微信

# 功能页面

>+ [管理插件](../home/i.html ":ignore e小天|个人中心")

>+ [展示页面](../app/demo.html ":ignore e小天|展示页面")

>+ [功能测试](../app/test.html ":ignore e小天|功能测试")


# 应用开发
>+ 插件应用通过websocket进行连接通讯
>+ 数据为json格式
>+ 开发者交流群:321620171

## 应用开发

>+ [创建应用](../home/app.html ":ignore e小天|创建应用")
>+ 框架支持启动命令行运行应用,如 node,python,java 等
>+ 框架启动DLL类型应用时,会执行应用的 WxExt_Run 方法
>+ 应用启动后在第一时间连接框架,否则会关闭应用

### web应用示例

关键词回复应用 https://github.com/wxext/ext-nodejs-keyhelp
web复读机应用 https://github.com/wxext/ext-js-demo

### dll应用开发

```
//应用导出Run方法即可
extern "C" __declspec(dllexport) void __cdecl CN_WXEXT_RUN(void) {
    //执行应用启动代码
}
//无法导出函数的,以下形式创建对应的Run方法
namespace CN.WXEXT
{
    public class WxExtApp
    {
        public void CN_WXEXT_RUN()
        {
            //执行应用启动代码
        }
    }
}
```

>+ 应用启动后,通过websocket连接即可,连接链接为
>+ ws://127.0.0.1:8202/wx?name=应用名称&key=连接密钥
>+ 应用名称和连接密钥可在启动参数中取得(程序内获取应用的启动参数),参考 关键词回复应用
>+ 测试应用会在应用目录下写入key,应用名称为www开头时,可通过http获取密钥
>+ 测试应用可打开WxExtApp.exe查看密钥

### web应用示例
```
应用中心搜索www
输入密钥:123按回车添加应用
到个人中心启动应用
此时可以通过http获取ws连接密钥
http://127.0.0.1:8203/ext/www/key.ini

```


### 创建应用
```
应用运行填写示例:
运行程序 cmd  主程序 /c node app.js
说明:通过cmd执行node app.js命令

运行程序 node  主程序 app.js
说明:通过node程序执行app.js

两种填法都可以实现运行app.js应用
```

## 调试应用
把安装目录下的 WxExtApp.exe 复制到需要调试的开发目录,打开即可调试
调试之前在运营中心启动对应的调试应用,WxExtApp刷新获取调试信息

## 应用通讯
>+ 应用通过websocket连接接收消息和发送消息,消息为json格式
>+ 登录多个微信时,用pid来选择，默认情况下(0使用第一个，-1操作全部)

### 软件配置
>+ 该方法返回所有配置项,部分配置可传入修改,重启软件生效
```
{
    "method": "setConfig",
    "log": 0,//1开启日志
    "allowNet": 1,//1开启外网,已新增key和ip白名单验证,可安全使用
}
```
### 应用配置
>+ 消息日志,插件日志,http推送日志,都有单独的配置.特别的:无论是否开启日志,都会记录错误日志
```
{
    "method": "setExt",
    "log": 0,//1开启日志
    "name": "wxext",//插件名称
}
```
### 启动功能
>+ 0取当前第一个微信，-1新启动一个微信
```
{
    "method": "run",
    "pid": 0
}
```
### 抖动微信
>+ 将微信抖动置顶
```
{
    "method": "show",
    "pid": 0
}
```
### 点击登录
>+ 点击微信启动时的登录页面
```
{
    "method": "clickLogin",
    "pid": 0
}
```
### 跳转二维码
```
{
    "method": "gotoQr",
    "pid": 0
}
```
### 退出登录
```
{
    "method": "loginOut",
    "pid": 0
}
```
### 结束进程
```
{
    "method": "kill",
    "pid": 0
}
```
### 连接列表
```
{
    "method": "list",
    "pid": 0
}
```
### 插件列表
```
{
    "method": "ext",
    "action": "list",
    "pid": 0
}
```
### 获取登录信息
```
{
    "method": "getInfo",
    "pid": 0
}
```

### 获取通讯录
>+ 包括好友、群聊、公众号
```
{
    "method": "getUser",
    "pid": 0
}
```
### 获取群列表
>+ 包括好友、群聊、公众号
```
{
    "method": "getGroup",
    "pid": 0
}
```
### 获取群成员
>+ 群内昵称，列表第一个为群主
```
{
    "method": "getGroupUser",
    "wxid": "23942162341@chatroom",
    "pid": 0
}
```
### 设置群公告
>+ 会自动添加@所有人
```
{
    "method": "setRoomAnnouncement",
    "wxid": "24288152652@chatroom",
    "msg": "朋友一生一起走",
    "pid": 0
}
```
### 设置备注
```
{
    "method": "setRemark",
    "wxid": "wxid_vw2prmx8xv5n22",
    "msg": "落落",
    "pid": 0
}
```
### 群踢人
```
{
    "method": "delRoomMember",
    "wxid": "4429337922@chatroom",
    "msg": "wxid_vw2prmx8xv5n22",
    "pid": 0
}
```
### 群拉人
```
{
    "method": "addGroupMember",
    "wxid": "4429337922@chatroom",
    "msg": "wxid_vw2prmx8xv5n22",
    "pid": 0
}
```
### 设置群名称
```
{
    "method": "setRoomName",
    "wxid": "24288152652@chatroom",
    "msg": "hello",
    "pid": 0
}
```
### 退出群聊
```
{
    "method": "quitChatRoom",
    "wxid": "23177484571@chatroom",
    "pid": 0
}
```
### 群邀请发送
```
{
    "method": "sendGroupInvite",
    "wxid": "4429337922@chatroom",
    "msg": "wxid_vw2prmx8xv5n22",
    "pid": 0
}
```
### 查头像
```
{
    "method": "getUserImg",
    "wxid": "wxid_vw2prmx8xv5n22",
    "pid": 0
}
```
### 查昵称信息
```
{
    "method": "getUserInfo",
    "wxid": "wxid_vw2prmx8xv5n22",
    "pid": 0
}
```
### 获取数据库列表
```
{
    "method": "getDbName",
    "pid": 0
}
```
### 执行数据库语句
```
{
    "method": "runSql",
    "db": "MicroMsg.db",
    "sql": "SELECT name FROM sqlite_master WHERE type='table' ORDER BY name;",
    "pid": 0
}
```


### 发送文本消息
>+ 需要艾特人时，传入atid
>+ 多人艾特用|分割，同时msg要对应多个艾特文本
```
{
    "method": "sendText",
    "wxid": "filehelper",
    "msg": "https://www.baidu.com/",
    "atid": "",
    "pid": 0
}
艾特消息
{
    "method": "sendText",
    "wxid": "23942162341@chatroom",
    "msg": "@昵称1 @昵称2 艾特消息",
    "atid": "wxid_xxx1|wxid_xxx2",
    "pid": 0
}
```
### 转发图片
```
{
    "method": "sendImage",
    "wxid": "filehelper",
    "img": "图片本地路径",//自动下载的路径通过xmlinfo推送来获取
    "imgType": "file",
    "pid": 0
}
```

### 发送动态表情
```
{
    "method": "sendEmoji",
    "wxid": "filehelper",
    "path": "C:\\Users\\hello.gif",//本地路径
    "pid": 0
}
```
### 转发动态表情
```
{
    "method": "sendEmojiForward",
    "wxid": "filehelper",
    "xml": "type=47收到的msg中的xml",//参考接收到的xml
    "pid": 0
}
```
### 转发文章链接小程序
```
{
    "method": "sendAppmsgForward",
    "wxid": "filehelper",
    "xml": "type=49收到的msg中的xml",//参考接收到的xml
    "pid": 0
}
```
### 发送语音通话
>+ 紧急事件可以通过发送语音通话实现
```
{
    "method": "callVoipAudio",
    "wxid": "wxid_vw2prmx8xv5n22"
}
```
### 清空聊天记录
>+ 长时间运行建议清空聊天记录
```
{
    "method": "ClearMsgList"
    "pid": -1//全部
}
```
### 其他
>+ lazy
```
向应用发送请求(应用需要回应请求)
{
    "method": "e",
    "name": "测试插件",
    "xxx","xxx"
}
发起通知(同时向ws和http推送type = 840的消息)
{
    "method": "extNotify",
    "xx": "xx",
    "xxx": "xxx"
}
获取文件数据
{
    "method": "getfile",
    "path": "C:\\Users\\....7fadd4bfe.dat"
}
保存数据(可选type:base64,hex,url,默认为保存文本)
{
    "method": "savefile",
    "path": "hello.txt"
    "data": "hello world"
}
保存图片
{
    "method": "saveimg",
    "type": "url"
    "data": "https://www.baidu.com/img/flexible/logo/pc/result.png"
}
同意好友
{
    "method": "agreeUser",
    "encryptusername": "收到的xml中获取",
    "ticket": "收到的xml中获取",
    "scene": 收到的xml中获取
}
接收转账收款
{
    "method": "agreeCash",
    "wxid": "对方wxid",
    "transferid": "收到的xml中获取",
    "pid": 0
}
忙时段设置图片等不自动下载
有三个时间段可以单独设置
flag=1 2 3 分别设置三个时间段
flag=0同时设置3个时间段
请求后会返回三个时间段
设置成 00:00-00:00 24小时自动下载
{
    "method": "downrange",
    "flag": "1",
    "data": "18:00-23:15"
}
网络获取详细信息,昵称 头像 是否好友拉黑
{
    "method": "netGetUser",
    "wxid": "wxid_vw2prmx8xv5n22|wxid_vryy2hqz4mv212",
    "pid": 0
}
### HTTPKEY
key计算方式:sha1("httpkey|wxext.cn|MachineName|密码")
MachineName为本机机器名,密码初始为空
本地请求带上referer可免key
修改key
{
    "method": "key",
    "old": "",//旧密码,默认为空
    "pwd": "",//设置新密码
    "ips": "127.0.0.1"//设置ip白名单
}
```
## 事件通知
>+ 设置通知地址后会将事件推送到指定地址  [去设置](../app/settings.html ":ignore e小天|设置中心")
>+ 可以推送到php等服务地址接收消息，然后通过http(需ip白名单带key请求)回复发送消息
>+ 插件通过websocket实时接收消息
>+ 每一个事件都有一个 type 字段表示含义

```
newmsg  type(1,3,34,43,37,47,48,49,10000) 微信消息
getchatroommemberdetail 701             群成员信息更新（new为1表示新成员，这里有邀请人id）
chatroommemberAdd   702             群成员增加（只有wxid）
chatroommemberSub   703         群成员减少（只有wxid）
getcontact  704                 联系人信息更新
tenpay  705                 收款结果
verifyuser  706             好友验证结果
createchatroom  707         创建群聊结果
xmlinfo 708                 对应xml资源下载到本机事件

登录相关信息通知
info    flag(open,qrchange,auth,login,logout)
auth    720                 
open    721
qrchange    723
login   724  登录,可以根据time过期时间判断有没有自动授权
logout  725
expired 729 授权到期前半小时通知/主动请求未授权通知

callVoipAudio  726  发起电话
callVoipAudio  727  挂断电话
callVoipAudio  728  接通电话

mmsns 731 朋友圈红点点
mmsns 732 新朋友圈推送
mmsns 733 朋友圈列表


exterr  802 插件连接断开通知
exterr  803 微信连接断开通知
exterr  804 http推送状态连续出错超过20次被停止推送
tips  810   系统提示点击确定通知
840   插件通知消息

应用状态 -1未安装 0已安装 1正在初始化 2正在运行 3运行错误 4停止运行
推送状态 1停止 2运行 3错误
```