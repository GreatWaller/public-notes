# Linux 命令行

#### 监测程序

##### **ps**

ps -ef 显示所有进程

```bash
$ ps -ef
UID        PID  PPID  C STIME TTY          TIME CMD
root         1     0  0 11:51 ?        00:00:00 /init
root        10     1  0 11:51 tty1     00:00:00 /init
xuan        11    10  0 11:51 tty1     00:00:00 -bash
xuan       271    11  0 14:32 tty1     00:00:00 ps -ef
```



![image-20200910143334638](C:\Users\Xuan\AppData\Roaming\Typora\typora-user-images\image-20200910143334638.png)

ps -l

```bash
$ ps -l
F S   UID   PID  PPID  C PRI  NI ADDR SZ WCHAN  TTY          TIME CMD
0 S  1000    11    10  0  80   0 -  4231 -      tty1     00:00:00 bash
0 R  1000   272    11  0  80   0 -  4271 -      tty1     00:00:00 ps
```



![image-20200910143519490](C:\Users\Xuan\AppData\Roaming\Typora\typora-user-images\image-20200910143519490.png)

##### top

```bash
$ top
top - 14:43:28 up  2:51,  0 users,  load average: 0.52, 0.58, 0.59
Tasks:   4 total,   1 running,   3 sleeping,   0 stopped,   0 zombie
%Cpu(s):  2.7 us,  1.8 sy,  0.0 ni, 95.0 id,  0.0 wa,  0.5 hi,  0.0 si,  0.0 st
KiB Mem :  8293160 total,  2692552 free,  5371256 used,   229352 buff/cache
KiB Swap: 25165824 total, 24642664 free,   523160 used.  2788172 avail Mem

  PID USER      PR  NI    VIRT    RES    SHR S  %CPU %MEM     TIME+ COMMAND
  274 xuan      20   0   17620   2044   1508 R   0.3  0.0   0:00.19 top
    1 root      20   0    8936    312    268 S   0.0  0.0   0:00.07 init
   10 root      20   0    8944    228    180 S   0.0  0.0   0:00.00 init
   11 xuan      20   0   16924   3644   3544 S   0.0  0.0   0:00.67 bash
```

> 一般平均负载load average 超过2，说明系统比较繁忙

![image-20200910144703647](C:\Users\Xuan\AppData\Roaming\Typora\typora-user-images\image-20200910144703647.png)

![image-20200910144722459](C:\Users\Xuan\AppData\Roaming\Typora\typora-user-images\image-20200910144722459.png)

f 排序

d 修改轮询间隔

##### kill

kill 3940

kill -s HUP 3940

##### killall

killall http*



#### 监测磁盘空间

##### mount

mount -t type device directory

##### umount

umount [directory | deivce]

##### df

df -h

##### du

du -sh

#### 处理数据文件

##### sort

sort -n file 

sort -t ':' -k 3 -n /etc/passwd

> du -sh * | sort -nr

##### grep

grep [options] pattern [file]

-v 反向

-n 显示行号

-c 匹配行数

grep [tf] 正则

##### tar

tar -cvf test.tar test/ test2/

tar -tf test.tar 只列出不提取

tar -xvf test.tar 提取

-z 重定向到gzip

#### Shell 

##### jobs -l 后台进程

##### coproc { sleep 10; } 协程-》子shell, 同时进入后台模式

#### 环境变量

##### printenv

printenv HOME

echo $HOME

##### set 全局、局部、用户定义变量

##### 设置局部变量 

```bash
$home=/home/xuan
```



##### 设置全局环境变量

```bash
$my_global=global
$export my_global
```

##### 删除环境变量

unset my_global

##### 设置PATH环境变量

```bash
$PATH=$PATH:/xuan
```

只能持续到退出或重启系统

![image-20200910175315116](Linux 命令行.assets/image-20200910175315116.png)

##### 环境变量持久化

如果设置在/etc/profile, 系统升级即消失；

最好在/etc/profile.d目录中创建一个以.sh结尾的文件。

存储个人用户变量： $HOME/.bashrc



#### 文件权限

/etc/passwd

```
root:x:0:0:root:/root:/bin/bash
```

