# DingDing-Automatic-Clock-in
 by https://github.com/georgehuan1994/DingDing-Automatic-Clock-in

<img width="300" src="https://github.com/5iCruise/DingDing-Automatic-Clock-in/raw/master/图片/Screenshot_2020-10-29-19-29-35-361_org.autojs.autojs.jpg"/> <img width="300"  src="https://github.com/5iCruise/DingDing-Automatic-Clock-in/blob/master/图片/Scrennshot_20201231094431.png"/>

## 简介
钉钉自动打卡、远程打卡脚本，基于AutoJs+任务栏通知提醒功能（本案例采用Tasker），免Root

## 功能
- 定时自动打卡
- 远程指令打卡
- 自动回复打卡结果

## 工具
- AutoJs
- Tasker
- 网易邮箱大师*（暂未使用邮件通知）

## 原理
在AutoJs脚本中监听本机通知，并在Tasker中创建定时任务发出打卡通知，或在另一设备上发送消息到本机，即可触发脚本中的打卡进程，实现定时打卡和远程打卡。

## 脚本（以上传本Repo）
```javascript
/*
 * @Author: George Huan
 * @Date: 2020-08-03 09:30:30
 * @LastEditTime: 2021-01-27 09:36:39
 * @Description: DingDing-Automatic-Clock-in (Run on AutoJs)
 * @URL: https://github.com/georgehuan1994/DingDing-Automatic-Clock-in
 */

略

```

## 使用方法
### AutoJs
下载：[Auto.js 4.1.1a Alpha2-armeabi-v7a-release](https://icruisedata.lanzous.com/iVeD6l0s2cd "Auto.js 4.1.1a Alpha2-armeabi-v7a-release")  密码: 84it

AutoJs是安卓平台上的JavaScript自动化工具 https://github.com/hyb1996/Auto.js

PC和手机连接到同一网络，使用 VSCode + Auto.js插件（在扩展中心搜索 "hyb1996"） 可方便的调试并将脚本保存到手机上

### Tasker
下载：[Tasker.11.14.apk](https://icruisedata.lanzous.com/ieVkzl107fe "Tasker.11.14 适用于5.0版以上") 密码:63e6

<img width="270" height="585" src="https://github.com/5iCruise/DingDing-Automatic-Clock-in/blob/master/图片/截图_004.jpg"/>

1. 添加一个 "通知" 操作任务，通知标题修改为 "定时打卡"，通知文字随意，通知优先级设为 1

2. 添加两个配置文件，使用日期和时间作为条件，分别在上班前和下班后触发

或者下载本repo下的两个“上班打卡.prf”和“下班打卡.prf” XML配置文件，导入到Tasker中使用

### 远程打卡（未试用）
回复标题为 "打卡" 的邮件，即可触发打卡进程

回复标题为 "考勤结果" 的邮件，即可查询最新一次打卡结果

### 暂停/恢复定时打卡
回复标题为 "暂停" 的邮件，即可暂停定时打卡功能（仅暂停定时打卡，不影响远程打卡功能）

回复标题为 "恢复" 的邮件，即可恢复定时打卡功能

### 注意事项
- 首次启动AutoJs时，需要为其开启无障碍权限

- 此脚本会自动适配不同分辨率的设备，但AutoJs对平板的兼容性不佳，不推荐在平板设备上使用

- AutoJs、Tasker可息屏运行，但需要在系统设置中开启通知亮屏

- 为保证AutoJs、Tasker进程不被系统清理，可调整它们的电池管理策略、加入管理应用的白名单，为其开启前台服务、添加应用锁

- 虽然脚本可执行完整的打卡步骤，但推荐开启钉钉的极速打卡功能，在钉钉启动时即可完成打卡，应把后续的步骤视为极速打卡失败后的保险措施

## 更新日志
### 2021-01-27
临时处理AutoJs监听线程无法停止的问题：在子线程开始前，调用threads.shutDownAll()，避免线程被重复开启。

AutoJs长时间运行后会出现这个问题（大概10天左右）

具体表现为：通知不能被正常监听，停止并重新运行脚本后，一条通知被多次打印

当出现这个情况时，请重启手机

### 2021-01-15

针对钉钉6.0版本进行调整：

1. 取消了 从消息界面进入工作台 以及 从工作台进入考勤打卡界面 这两个过程

2. 启动并成功登录钉钉后，直接使用URL Scheme拉起考勤打卡界面

### 2021-01-08

修复：通知过滤器报错

### 2020-12-30

优化：现在可以通过邮件来 暂停/恢复 定时打卡功能，以应对停工停产，或其他需要暂时停止定时打卡的特殊情况

### 2020-12-04

优化：打卡过程在子线程中执行，钉钉返回打卡结果后，直接中断子线程，减少无效操作

### 2020-10-27

修复：当钉钉的通知文本为null时，indexOf()方法无法正常执行

### 2020-09-24

优化：若无法进入考勤打卡界面，则使用URL Scheme直接拉起考勤打卡界面

```javascript
function attendKaoqin(){
    var a = app.intent({
        action: "VIEW",
        data: "dingtalk://dingtalkclient/page/link?url=https://attend.dingtalk.com/attend/index.html" // 在后面加上 ?CorpId=************
      });
      app.startActivity(a);
      sleep(5000)
}
```

#### 获取URL的方式如下：

1. 在PC端找到 “智能工作助理” 联系人

2. 发送消息 “打卡” ，点击 “立即打卡” 

3. 弹出一个二维码。此二维码就是拉起考勤打卡界面的 URL Scheme ，用自带的相机或其他应用扫描，并在浏览器中打开，即可获得完整URL Scheme

4. 无需使用完整URL，将`?CorpId=***` 拼接到 `dingtalk://dingtalkclient/page/link?url=https://attend.dingtalk.com/attend/index.html` 的后面

5. 仅使用 `dingtalk://dingtalkclient/page/link?url=https://attend.dingtalk.com/attend/index.html`，也可以拉起旧版打卡界面，钉钉会自动获取主企业的CorpId，并跳转到新版打卡界面

### 2020-09-11

1. 将上次考勤结果储存在本地

2. 将运行日志储存在本地 /sdcard/脚本/Archive/

3. 修复在下班极速打卡之后，重复打卡的问题

### 2020-09-04

将 "打卡" 与 "发送邮件" 分离成两个过程，打卡完成后，将钉钉返回的考勤结果作为邮件正文发送

### 2020-09-02

钉钉工作台界面改版（新增考勤打卡的快捷入口）。无法通过 "考勤打卡" 相关属性获取控件，改为使用 "去打卡" 文本获取按钮。若找不到 "去打卡" 按钮，则直接点击 "考勤打卡" 的屏幕坐标

## -EOF-
