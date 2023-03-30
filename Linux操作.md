# linux系统安装
1. centos、ubuntu(wsl)
2. 虚拟机快照

# 基本操作
1. 目录结构：linux系统只有一个根目录"/"(不同于windows可以拥有多个盘符"C:\")
2. command [-options] [parameter]
3. HOME目录：默认工作目录"/home/username"
4. 相对路径、绝对路径、特殊路径符(. .. ~)
5. 管道符 ls -l ~ | grep "test"
6. 反引用符`` , 重定向符> >>

## 基础命令
1. ls [-a -l -h] [Directory]  
   -a : -all 隐藏文件(.开头)  
   -l : 竖向展示  
   -h : 易于阅读的形式展示文件大小
2. cd [Directory] / pwd
3. mkdir [-p] [Directory]  需要修改权限
4. touch / cat / more [Directory]   (quit)
5. cp [-r] [parameter] / mv [parameter] / rm [-r -f] [parameter]  
   rm 支持通配符
6. which [Command] / find [Dir] -name [Name] / find [Dir] -size +|-n[kMG]
7. grep [-n] [key] [Dir] / wc [-c -m -l -w] [Dir]
8. echo [content]  
   echo "here are : `pwd`" > test.txt    
9. tail [-f -num] [Dir]

## 用户和权限
1. su [-] [user]/ exit
2. sudo visudo(vi /etc/sudoers)配置普通用户权限  username ALL = (ALL)   NOPASSWD: ALL
3. groupadd / groupdel
4. useradd [-m -g -d] / userdel [-r] / id [user] / usermod -aG [goup] [user]
5. getent [passwd group]
6. 权限信息[-dl] [r-][w-][x-] [r-][w-][x-] [r-][w-][x-]
7. chmod [-r] 权限 文件或文件夹  例如 chmod -r u=rwx,g=rx,o=x test  
   权限数字序号rwx(0b111)
8. chown [-r] [usr][:][group] 文件或文件夹

# 实用操作
1. ctrl + C/D/R  history !  
   ctrl + a/e/<-/->
2. apt/yum [-y] [install/remove/search] [name]
3. system start | stop | status | enable | disaable 服务名
4. ln -s
5. date [-d] [+格式化字符串]
6. ps [-e -f]   ps -ef | grep "xxx"
7. kill [-9] 进程号
8. top , df , iostat , sar -n DEV
9. ~/.bashrc   ， /etc/profile   ， source


# g++

- name.cpp -> g++ -E name.cpp -o name.i 预处理  
- name.i   -> g++ -S name.i 编译  
- name.s   -> g++ -c name.s 汇编  
- name.o   -> g++ name.o 链接  
- a.out    -> ./a.out  

## 动态链接(.so，.dll)与静态链接

# 选择

netstat 

top (CPU), ps

内存泄漏 (swap, vmstat, )

iostat

vim (ndd, A, a ...)

kill , nohup , &, tail, grep , | 

