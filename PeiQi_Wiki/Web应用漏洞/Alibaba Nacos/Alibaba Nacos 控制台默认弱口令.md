# Alibaba Nacos 控制台默认弱口令

## 漏洞描述

Alibaba Nacos 控制台存在默认弱口令 **nacos/nacos**，可登录后台查看敏感信息

## 漏洞影响

> [!NOTE]
>
> Alibaba Nacos

## 漏洞复现

发送如下请求

![](image/nacos-12.png)

返回200说明成功登录

## Goby & POC

> [!NOTE]
>
> Alibaba Nacos 控制台默认弱口令

![](image/nacos-13.png)