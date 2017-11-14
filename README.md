# 日志分析
```
打包后对linux glibc版本有要求
centos6 环境glibc 版本一般是2.12
centos7 环境glibc 版本一般是2.17
ubuntu没有试过

查看glibc版本命令
rpm -qa | grep glibc
rpm -qi glibc
连接地址：http://blog.sina.com.cn/s/blog_75acbe0b0101596n.html
```

nginx 配置加入：
```
log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent $request_time "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';
```
修改http配置：access_log  /home/wwwlogs/web.log main;


log操作配置
```
Options:
  -h, --help            show this help message and exit
  -f FILE_PATH, --file-path=FILE_PATH
                                 Description: Specific a destination File Path
  -t OPERATION_TYPE, --operation-type=OPERATION_TYPE
                                 Description: default ip 
                                 -t ip :   Access Statistics ip    
                                 -t page:  Access Statistics page
                                 -t size:  page size
                                 -t timeout:  request time  
                                 -t status:   website status 
                                 -t xforward:Statistical real ip address
  -n SHOW_ROWS, --show-rows=SHOW_ROWS
                                 Description: default none
  -p PAGE_BYTES, --page-bytes=PAGE_BYTES
                                 Description: default page bytes  greater than
                        200kb
  -r REQUEST_TIME, --request-time=REQUEST_TIME
                                 Description: default request time  greater
                        than 2s
  -l TIME_LOCAL, --time-local=TIME_LOCAL
                                 Description: default current time eg:31/Oct
```  
  
    案例：
 ```
   [root@localhost dist]# ./log  -f access_accountbackend.log  -t ip -l 01/Nov -n 10 #显示11.1日访问前10的ip
     28 100.116.190.120
     27 100.116.190.107
     25 100.116.190.76
     24 100.116.190.32
     24 100.116.190.18
     23 100.116.190.48
     23 100.116.190.33
     23 100.116.190.11
     22 100.116.190.61
     22 100.116.190.4
 [root@localhost dist]# ./log  -f access_accountbackend.log  -t timeout -r 0.1  -n 5  #展示当天页面访问用时超过0.1秒前5个所有页面
    440 /privilege/user
    170 /
      7 /crontab/asynsendmess
      4 /privilege/account/userinfo
      3 /crontab/synccompanyleads
[root@localhost dist]# sudo ./log  -f access_accountbackend.log  -t xforward  -l 01/Nov -n 5 #展示访问真实ip前5个
   1001 123.55.85.181
    317 228.244.142.151
     84 221.93.181.110
     62 111.165.175.113
     49 223.223.194.71
```
