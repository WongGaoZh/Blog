# 
### linux命令第二部分(硬件相关)

| | 名称 | 命令  | 描述 |
|-----|-----|-----|----|
| |查看linux版本 | cat /etc/issue     cat /etc/redhat-releas| |
| |查看内核|uname -a||
| |切换用户|su -username||
| |修改文件权限|chmod 777 file.java|-rwxrwxrwx r表示读，w表示写，x表示可执行|
|*|查看系统当前时间|date||
|*|查看机器内存大小|cat /proc/meminfo &#124; grep MemTotal||
|*|查看GPU信息|nvidia-smi||
| |查看指令集| cat /proc/cpuinfo |grep -E 'avx2|avx'||
| |查看cpu核数|lscpu|grep  'CPU(s)'||
| |查看显卡的个数| lspci |grep -i nvidia||
| |检查主机名| ifconfig||
| |查看防火墙状态|  systemctl status firewalld| |
| |关闭防火墙| systemctl stop firewalld||
| |永久关闭防火墙| systemctl disable firewalld||
| |查看服务状态|service  iptables status||
| |开启服务状态|service  iptables start||
| |关闭服务状态|service  iptables stop||
| |||
| |机器生成密钥|ssh-keygen|
| |配置免密登入| (生成密钥文件)ssh-copy-id root@${机器ip}||



---
### 磁盘相关

| | 名称 | 命令  | 描述 |
|-----|-----|-----|----|
| |查看磁盘挂载|lsblk -f ||
| |挂载磁盘|mount /dev/xxx /盘符名||
| |挂载永久生效|vim /etc/fstab 然后 mount -a||
| |查看硬盘大小|df -h||




