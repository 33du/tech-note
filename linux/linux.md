LANG=en_US 修改语言
echo $LANG 显示当前语言

根目录（/）下的：
/opt 装第三方软件，或/usr/local
/bin 普通用户用到的指令，与开机、单人维护模式有关，如/bin/ls *
/boot
/dev devices，硬盘、外设等 *
/etc 配置文件 
	/etc/init.d： 系统开机的时候加载服务的scripts的摆放地点
	/etc/X11/： X Window有关的各种配置文件 *
/home ~user表示该用户的home目录
/lib 开机时用到的函式库，/bin和/sbin中指令用到的函式库 *
/media 可移除装置
/mnt 临时挂载
/root 管理员的home目录
/sbin 开机、修复、还原系统所需要的指令，系统管理员才会用到 *
/srv service，如/srv/www
/tmp 临时存放
*： 必须与根目录在同一个分割槽中，因为开机时只挂载根目录，而它们与开机有关

/usr 系统自带软件放置处 
	apt install时/usr/bin或/usr/local/bin（即PATH）中生成binary，所以可直接运行
	/usr/bin 大多数用户可使用指令，与开机无关
	/usr/local 自己下载的软件
	/usr/sbin 非系统正常运作需要的系统指令
	/usr/src 存放source code
/var 可变动的，与系统运作过程有关，如cache，log file等 
	/var/mail，/var/run (程序的PID)
	/var/log 系统注册表

/etc/passwd 账号
/etc/shadow 密码
/etc/group 组

su - username 变更user

ls 	-al 查看权限、i-node、修改日期、大小等 （-a： 显示隐藏文件） 
	-d 仅列出目录本身，不包括它含有的
权限： drwxr-x--- 
第一个表示文件类型： d 目录  - 档案（包括纯文本、binary、data）  l link file（类似快捷方式）
后面三个为一组： 拥有者、同组、其他人

对于dir： r只能查看含有的文件名，有x权限才可进入该目录。w可以删除其中的文件，而文件本身有w权限却只能修改，不能删除
文件有x权限才可执行，相当于.exe之类

chgrp ：改变档案所属群组 chgrp [-R] groupname filename/dirname
chown ：改变档案拥有者
chmod ：改变档案的权限 chmod [-R] 770 file / chmod u=rwx,g=rx,o=r file

-R： recursive，即同目录下的所有文件都变更

数字表示权限： r=4, w=2, x=1
owner = rwx = 4+2+1 = 7
group = rwx = 4+2+1 = 7
others= --- = 0+0+0 = 0

cat 读出一个文档内容
nano 编辑

执行文件：如不在正规的执行文件目录中（如*/bin），必须指定在本目录下，即./run.sh，或用绝对路径

pwd： 显示当前目录，加-P表示link到的真实地址
mkdir 多层目录：mkdir -p test1/test2/test3/test4
rmdir -r 删除目录中所有东西
cd - 回到上一个目录

cp -s bashrc bashrc_slink #soft link
cp -l bashrc bashrc_hlink #hard link

执行文件路径： $PATH 含有各种/bin，输入一个指令（如ls）时会在$PATH的所有目录下找文件名为ls的可执行文件
PATH="$PATH":/root 把/root加入path中
