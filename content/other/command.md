### 1、常用命令
```bach
//查看进程
$ tasklist

//查看指定端口的占用情况
$ netstat -aon|findstr '9050'

//结束进程
$ taskkill /pid pid号 /f /t
//或
$ ntsd -c -p q PID号

$ lsof -i //用以显示符合条件的进程情况，lsof(list open files)是一个列出当前系统打开文件的工具，
$ lsof -i:端口号 // 用于查看某一端口占用情况，比如查看22端口使用情况 lsof -i:22
$ netstat -tunlp // 用于显示tcp,udp的端口和进程等相关情况
$ netstat -tunlp|grep 端口号 // 用于查看指定端口号的进程情况
```
### 2、
