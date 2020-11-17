# 锐捷RG-UAC统一上网行为管理审计系统存在账号密码信息泄露

## 漏洞描述

锐捷RG-UAC统一上网行为管理审计系统存在账号密码信息泄露,可以间接获取用户账号密码信息登录后台

## 影响版本

> [!NOTE]
>
> 锐捷RG-UAC统一上网行为管理审计系统

## FOFA

> [!NOTE]
>
> title="RG-UAC登录页面"

## 漏洞复现

来到登录页面

![](image/ruijie-1.png)

按F12查看源码,可以发现账号和密码的md5形式

![](image/ruijie-2.png)

解密md5得到密码后即可登录系统

![](image/ruijie-3.png)

![](image/ruijie-4.png)