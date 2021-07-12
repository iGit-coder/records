##### 1. 用户/用户组概念

```shell
#创建用户 vbird1/vbird2/vbird3
useradd vbird1
useradd vbird2
useradd vbird3
#创建用户组
groupadd team
#查看用户uid、gid
id vbird1
#查看用户/用户组详细信息
grep vbird1 /etc/passwd /etc/shadow /etc/group
grep team /etc/group /etc/gshadow
finger vbird1 
#修改用户/用户组信息
usermod -G team vbird1
groupmod -n team0 team
#删除用户/用户组
userdel -r vbird
groupdel team0
```

##### 2. 文件权限概念

```shell
#创建文件
touch filename
#查看文件权限
ll | grep filename
#更改文件所有者、用户组
chown user0 filename
chgrp subusers filename
#更改文件读写执行权限4 2 1
chmod 777 filename
#################
#创建目录
mkdir dirname
#更改文件所有者、用户组
chown user0 dirname
chgrp user0 dirname
#更改文件读写执行权限
chmod 777 dirname
#################
#验证SGID团队合作的作用
#设置目录为SGID 2[SUID 4 SBIT 1]
chmod 2777 dirname
#查看权限
ll | grep dirname
#使用其他用户进入目录
su - otheruser
#切换到dirname目录，需要其他用户拥有执行权限
cd dirname
#查看用户当前有效用户组，发现输出的依然是原来的有效用户组
groups
#创建文件
touch file.sh
#查看权限,发现文件的用户组为目录dirname的用户组
ll | grep file.sh
###通过以上验证，当目录设置为SGID时，其他用户拥有r权限可以ls目录列表，拥有
###x权限可以cd到目录成为工作目录，拥有w权限则可以在目录里创建文件，并且创建
###的文件的用户组为当前工作目录的用户组而不是用户所在用户组。显然这样的目录
###方便协同工作，因为这能够确保在该目录下的文件的所属用户组都是同一个用户组。
###补充1：创建这样的命令并不是让所有人都可以享受，而是让目录用户组包含的用户
###在该目录下进行的创建的文件所属用户组都为目录用户组，方便其他同组人协同合作，
###因为有的时候，用户的有效用户组可能会变，这样做用户切换到该目录下就不用修改自己
###的有效用户组就可以创建属于该用户组的文件了。




```

##### 3. 解压缩命令

```shell
#范例一：将/etc/man_db.conf复制到/tmp下，并进行压缩
cd /tmp
cp /etc/man_db.conf .
gzip -v man_db.conf
ll /etc/man_db.conf ./man_db.conf*
#范例二：查看压缩后的文件内容，但不解压
zcat man_db.conf.gz #bzip2时使用bzcat
#范例三：将刚才的压缩文件进行解压缩
gzip -d man_db.conf.gz
#bzip2和gzip用法相同
#tar几个简单且常用的命令
tar -jcv -f filename.tar.bz2 要压缩的文件或目录
tar -jtv -f filename.tar.bz2 
tar -jxv -f filename.tar.bz2 -C 与解压缩的目录
#范例四：备份/etc下的配置文件，权限等一并备份
tar -jpcv -f /root/etc.tar.bz2 /etc
#范例五：查询备份文件中的内容
tar -jtv -f /root/etc.tar.bz2
#范例六：将范例四备份的文件解压到指定文件夹
tar -jxv -f /root/etc.tar.bz2 -C /tmp
#范例七：将压缩文件中的单个文件解压出来
tar -jxv -f /root/etc.tar.bz2 etc/shadow
#范例八：打包目录但不含目录下某些文件或子目录
tar -jcv -f /root/system.tar.bz2 --exclude=/root/etc* \
> --exclude=/root/system.tar.bz2 /etc /root
#范例九：仅备份比某个文件新的所有文件
find /etc -newer /etc/passwd #查看比passwd新的文件
tar -jcv -f /root/etc.newer.then.passwd.tar.bz2 -newer-mtime='2021/04/02 22:47' /etc/*
```

##### 4. 正则表达式

```shell
#grep
grep -n 'the' demo.txt #输出文件中含the的行，显示行号
grep -vn 'the' demo.txt#输出文件中不含the的行，显示行号
grep -in 'the' demo.txt#输出文件中含the的行，不区分大小写，显示行号

grep -n 't[ae]st' demo.txt#输出文件中的行，行里面包括tast或test
grep -n '[^g]oo' demo.txt#输出文件中的行，行里面有oo但oo前面没有g
grep -n '[0-9]' demo.txt#输出文件中的行，行中有数字
grep -n '^the' demo.txt#输出文件中的行，行以the开头
grep -n '^[a-z]' demo.txt#输出文件中的行，行以小写字母开头
grep -n '\.$' demo.txt#输出文件中的行，行都是以.结尾
grep -n '^$' demo.txt#输出文件中的行，行为空行
grep -n 'g..d' demo.txt#输出文件中的行，行中有gxxd字符串，其中x为任意字符
grep -n 'o\{2\}' demo.txt#输出文件中的行，行中有‘oo’字符串
grep -n 'go\{2,5\}d' demo.txt#输出文件中的行，行中有‘good'or'goood'or'gooood'or'goooood'
grep -n 'go\{2,\}d' demo.txt#输出文件中的行，行中有’good'or'goood'or'goo...d'
```

| RE字符串  |                           意义                            |
| :-------: | :-------------------------------------------------------: |
|   ^word   |                      待查word在行首                       |
|   word$   |                      待查word在行尾                       |
|     .     |                代表'.'处一定有一个任意字符                |
|     \     |                         转义字符                          |
|     *     |          重复零个到无穷多个前一个字符,等价于{0,}          |
|     +     |          重复1个到无穷多个前一个字符，等价于{1,}          |
|    ？     |             重复0或1次前一个字符，等价于{0,1}             |
|  [list]   | 从字符集list中找到想要选取的字符，如a[afl]g = aag afg alg |
|  [n1-n2]  |                    从n1到n2范围内查找                     |
|  [^list]  |                      [list]反向查找                       |
| \\{n,m\\} |     连续n到m个前一个字符，\\{n,\\}代表从n到任意大于n      |



sed

```shell
nl /etc/passwd | sed '2,5d'#删除2-5行
nl /etc/passwd | sed '2,$d'#删除2-最后一行
nl /etc/passwd | sed '2a Drink tea or'#在第二行后面添加一行内容为Drink tea or
nl /etc/passwd | sed '2,5c No 2-5number'#将第二行到第五行替换为No 2-5number
nl /etc/passwd | sed -n '5,7p'#显示第五行到第七行
#查找替换 s/old/new/g
ifconfig ens33 | grep 'inet '| sed 's/^.*inet //g'|sed 's/netmask.*$//g' #只输出ip地址
#直接修改文件内容,将以.结尾的行改成以!结尾
sed -i 's/^.$/\!/g' demo.txt
#查看第2行数据
sed -n '2p' demo.txt
```

##### 5. shell

> - shell有多种，bash shell、csh shell、zsh shell和ksh shell等
>
> - bash shell功能：命令记忆、补全、别名和通配符等
>
> - 命令来源有shell内置和外置
>
> - shell环境变量与自定义变量区别：
>
>   1. 环境变量名一般大写如HOME、SHELL和PATH等，自定义变量小写
>
>   2. 环境变量在子进程中依然有效，自定义变量只在定义进程中有效
>
>      `将自定义变量转换为环境变量：export name`

- 自定义变量

> 定义变量时注意：变量名不能以数字开头、等号两端不能有空格、自定义变量一般小写或大小写混合

```shell
2name=Vbird#no
name = Vbird#no
name=Vbird#ok
name="Vbird's name"#ok
name='Vbird's name'#no
```

- 取出变量

```shell
#输出shell环境下的所有环境变量
env
#输出shell环境下的所有变量（包括环境变量和自定义变量）
set
#输出所有环境变量
export
#输出单个变量
echo $PATH 
echo ${PATH}
echo "${PATH}"#输出PATH具体值
echo '${PATH}'#输出${PATH}
cd /lib/modules/`uname -r`/kernel#使用命令获取需要的变量值
cd /lib/modules/$(uname -r)/kernel#等效上面
```

- 取消变量

```shell
uset name
```

- 语系变量

```shell
#查看linux支持多少种语系
locale -a
#查看当前使用语系
locale
```

- 键盘读取

  ```shell
  #从键盘中输入数据赋值给var
  read var
  #30秒内输入
  read -t 30 var 
  #输出提示语，并在30秒内输入
  read -p "Please keyin your name:" -t 30 name
  ```

- 声明类型

  ```shell
  #声明数组
  declare -a arr
  #数组赋值
  arr[1]=v1
  #声明整型
  declare -i var
  #声明变量为环境变量
  declare -x var
  #声明变量为只读
  declare -r var
  ```

- 限制创建文件大小

  > 单个文件大小与文件系统的块有关系，单个块越大，允许单个文件就越大。因为这涉及到数据存储分布的零散程度问题，
  >
  > 即对于大文件，如果块较小，那么存储单个单文件需要更多的块，这样使得数据存储分布较零散

  ```shell
  #限制创建文件最大不超过10M
  ulimit -f 10240
  ```

- 变量内容的删除、替代

  ```shell
  #输出变量path,#代表从前面开始删除， 符合的最短的字符，表示删除的起始位置为最前面的‘/‘,*代表零或任意多字符，kerberos/bin:表示以此结尾即删除的终点,path中第一个’kerberos/bin:‘
  echo ${path#/*kerberos/bin:}
  #两个#号表明从前面开始删除，符合的最长的字符，/表示删除的起始位置为最前面的‘/‘，*代表零或任意多字符，:表示以此结尾即删除的终点，path中最后一个’:‘
  echo ${path##/*:}
  
#如果username没定义则输出为root
  echo ${username-root}
  #如果username没定义或定义为“”，则输出为root
  echo ${username:-root}
  
  #如果oldstr不存在那么就将oldstr和newstr都设置为root
  newstr=${oldstr=root}
  
  #如果oldstr没有定义，则会输出无此变量
  var = ${oldstr?无此变量}
  ```
  
  