# 
## linux命令第二部分(文件相关)


| | 名称 | 命令  | 描述 |
|-----|-----|-----|------|
|*|查找文件|find / -name filename.txt| 根据名称查找/目录下的filename.txt文件|
|***|查找文件|locate  filename.txt| 根据名称查找的filename.txt文件,速度快|
|*|复制文件包含其子文件到自定目录|cp -r sourceFolder targetFolder||
|*|删除文件包含其子文件|rm -rf deleteFile||
|***| 查看文件内容|tail、cat、tac、head、echo||
| | 查看文件头10行 | head -n 10 example.txt||
| |查看文件尾10行 | tail -n 10 example.txt|| 
||||
|*|压缩文件|tar -czf test.tar.gz /test1 /test2||
|*|列出压缩文件列表|tar -tzf test.tar.gz||
|*|解压文件| tar -zxvf test.tar.gz||
|*|统计当前文件夹的文件的数量|ls |wc -l||



#### 查看文件内容-理论

使用tail查看

从第3000行开始，显示1000行。即显示3000~3999行

cat filename | tail -n +3000 | head -n 1000

分解:
tail -n 1000：显示最后1000行

tail -n +1000：从1000行开始显示，显示1000行以后的

head -n 1000：显示前面1000行

使用sed命令
sed -n '5,10p' filename 这样你就可以只查看文件的第5行到第10行。


cat主要有三大功能：

1、一次显示整个文件。 cat filename

2、从键盘创建一个文件 cat>filename  只能创建新文件，不能编辑已有文件

3、将几个文件合并为一个文件：cat file file2 > filename

`
参数
-n 或 --number 由 1 开始对所有输出的行数编号
-b 或 --number-nonblank 和 -n 相似，只不过对于空白行不编号
-s 或 --squeeze-blank 当遇到有连续两行以上的空白行，就代换为一行的空白行
-v 或 --show-nonprinting
例：
把 textfile1 的档案内容加上行号后输入 textfile2 这个档案里
cat -n textfile1 > textfile2
把 textfile1 和 textfile2 的档案内容加上行号（空白行不加）之后将内容附加到 textfile3 里。
cat -b textfile1 textfile2 >> textfile3
把test.txt文件扔进垃圾箱，赋空值test.txt
cat /dev/null > /etc/test.txt
注意：>意思是创建，>>是追加。千万不要弄混了。
`

tac (反向列示)
tac 是将 cat 反写过来，所以他的功能就跟 cat 相反， cat 是由第一行到最后一行连续显示在萤幕上，
而 tac 则是由最后一行到第一行反向在萤幕上显示出来！

echo "the echo command test!"
这个就会输出“the echo command test!”这一行文字！
echo "the echo command test!">a.sh


#### 查看文件内容-实战

比如查看那个错误:getTSsfwClzyLbList error,url is

tail -f catalina.out |grep 'getTSsfwClzyLbList error,url is'
如果打印不出来使用
tail -f log | grep --line-buffer xxx


查看某个文件下面的筛选条件(坑是 时间必须为日志里面有的)
显示行及其后面五行
cat catalina.out |grep  -A 5 'getTSsfwClzyLbList error,url is' 


查看某段时间的日志

sed -n '/开始时间日期/,/结束时间日期/p' all.log

按小时模糊查询
sed -n '/2019-10-24 21*/,/2019-10-24 22*/p' all.log

结合grep查询

sed -n '/2019-10-24 22:16:21/,/2019-10-21 20:16:58/p' all.log | grep POST


在access-log日志查找记录的时候 ,
用awk命令 : 
-F 对什么进行切分
awk -F"[ ]"  access-log.txt 


#### md5sum命令 
可以对比两个文件是否一致,不需要打开文件去挨行对照


#### 查找文件
推荐使用locate

```
       which  查看可执行文件的位置。
       whereis 查看文件的位置。 
       locate   配合数据库查看文件位置。
       find   实际搜寻硬盘查询文件名称。
which命令的作用是，在PATH变量指定的路径中，搜索某个系统命令的位置，并且返回第一个搜索结果。
也就是说，使用which命令，就可以看到某个系统命令是否存在，以及执行的到底是哪一个位置的命令。 


where is  查看bin下面有那些命令


TIPS：在用搜索指令时，请记得用sudo且cd到  /  目录下，这样才不会找漏文件，这个很重要！！！！
```


#### vi命令

```shell script
vi 命令下
set nu 显示行数
```