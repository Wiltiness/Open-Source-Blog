---
title: 配置Microsoft Entra ID平台以允许企业用户注册通行密钥
pubDatetime: 2026-04-16
description: 本文详细介绍了如何在 Microsoft Entra ID 平台中启用 FIDO2 安全密钥登陆策略，允许员工使用通行密钥实现无密码登录
draft: false
featured: false
tags: []
---
一开始，我是想要给我的Windows电脑配置Windows Hello Passkey来登录Outlook之类的M365应用，但是一直注册不成功，提示如下图所示：

\
![](Screenshot%202026-03-24%20140251.png)

(图源:[Microsoft Learn Document](https://learn.microsoft.com/en-us/answers/questions/5839154/end-user-unable-to-create-passkey-for-entra-id-acc))

### 登录到Microsoft Entra ID

然后，通过咕噜咕噜到处搜索了一番之后，查到需要去Microsoft Entra ID调整配置文件，步骤如下\
\
1.打开[Microsoft Entra ID](https://entra.microsoft.com/auth/login/),登录**全局管理员**账号\
![](%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202026-04-16%20111215.png)

2.进入主页

![](%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202026-04-16%20111315.png)

### 配置通行密钥策略

##### 常规密钥类型

3.选择"Entra ID → 身份验证方法",页面框架大致如下：\
![](%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202026-04-16%20111504.png)

注：若此前从未进行过配置，则"密钥 (FIDO2)"的"已启用"栏目会为"否"。

4.点击"密钥 (FIDO2)"，点击“配置”选项栏

![](image.png)\
原始状态应当不会有任何配置文件，要先勾选"允许自助服务设置"（允许用户在个人信息页面自行注册或者撤销PassKey）\
然后添加配置文件（如下图所示）：\
![](%E5%B1%8F%E5%B9%95%E6%88%AA%E5%9B%BE%202026-04-16%20112122.png)

这里的几个选项，填写基本标准如下：\
名称:自行填写,没有特殊意义\
"强制认证"选项:无需勾选（若勾选，则Google PassWord Manager/Authencator或iCloud KeyChain有可能会反复注册失败）

”通行密钥类型“复选框:存在Device-Bound和Synced两个选项，分别对应设备本地密钥(Windows Hello Passkey)/云同步密钥(iCloud/Google Authencator).你需要注册哪种类型的密钥就勾选哪一项，但是建议均勾选上

目标AAGUID：无需勾选，后文会提到

保存配置文件，重新选择"启用和定向"栏目，配置所生效的用户：

选择“添加用户”->“所有用户”，勾选刚才创建的配置文件

点击保存，常规密钥配置流程到此结束。

### （可选配置）Windows Hello Passkey

注释：Windows Hello Passkey存在多种情况，对于Windows 11 Enterprise版本，无需额外配置即可注册通行密钥，其他版本的Windows操作系统则需要进行该配置

重新回到配置文件页，新增一个配置文件，内容如下：

名称:自行填写,没有特殊意义\
"强制认证"选项:无需勾选

”通行密钥类型“复选框:存在Device-Bound和Synced两个选项，分别对应设备本地密钥(Windows Hello Passkey)/云同步密钥(iCloud/Google Authencator).你需要注册哪种类型的密钥就勾选哪一项，但是建议均勾选上

目标AAGUID：勾选，行为设置为Allow

"模型/提供程序 AAGUID&nbsp;"选项：添加AAGUID，选择Windows Hello (Preview)。

点击Save之后，确认有以下三项存在于“模型/提供程序 AAGUID”：

Windows Hello VBS Hardware Authenticator

Windows Hello Hardware Authenticator

Windows Hello Software Authenticator\
点击Save，重新选择"启用和定向"栏目，配置所生效的用户。选择“添加用户”->“所有用户”，勾选刚才创建的配置文件（可以同时存在多个配置文件，但不得相互冲突）

## 恭喜，此时你就可以去[M365 User Center](https://myaccount.microsoft.com/?ref=MeControl)注册通行密钥了
