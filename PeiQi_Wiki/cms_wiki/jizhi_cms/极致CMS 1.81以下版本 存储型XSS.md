# 极致CMS 1.81以下版本 存储型XSS

### 漏洞复现

登录管理员添加模块

![](image/jizhi-1.png)

注册用户

![](image/jizhi-2.png)

点击发布文章

![](image/jizhi-3.png)

在文章标题处插入xss payload

```<details open ontoggle= confirm(document[`coo`+`kie`])>```

当管理员访问时XSS成功

![](image/jizhi-4.png)



### 参考

[极致CMS代码审计](https://xz.aliyun.com/t/7861)