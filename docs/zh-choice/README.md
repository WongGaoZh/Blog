# 技术选型

## 权限控制技术和技术选型

spirng security 和 oauth2.0 还有shiro的各种区别和作用 

security是权限的框架, jwt 是是一套生成令牌的标准, shiro 权限框架, oauth2.0 是一种权限实现的标准

CAS 是Central Authentication Service ，中央认证服务，一种独立开放指令协议。CAS 是 耶鲁大学（Yale University）发起的一个开源项目，旨在为 Web 应用系统提供一种可靠的单点登录方法，CAS 在 2004 年 12 月正式成为 JA-SIG 的一个项目。开源的企业级单点登录解决方案

应用场景不一样: 

OAuth2用在使用第三方账号登录的情况(比如使用weibo, qq, github登录某个app)

JWT是用在前后端分离, 需要简单的对后台API进行保护时使用.(前后端分离无session, 频繁传用户密码不安全)


