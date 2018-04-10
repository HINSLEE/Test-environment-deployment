# Test-environment-deployment
# BOP-REPORT-打包-测试环境部署步骤

转至元数据起始 


虚拟机已创建，本地可直接连接，不允许修改root密码。 
主机名：gzmb-2p-38-AOF.mobiabc.com 
IP：172.24.2.38 
ssh端口号：22 
负责人：collin@mobisummer.com 
账号：root 
密码：IzmEoNKz 
cpu ：2 个 
内存：2 G 
硬盘：200G 
*********************** 禁止在虚拟机上自行添加ip！*******************

一、打包（以bop-gateway为例）
登录Jenkins，进入bop/bop-gateway，点击左侧的“Build with Parameters“；
选择对应的分支（brunch），version按“时间-名称”（如20180403-report_InfoIndex）命名，点击“开始构建”。打包完成。

二、测试环境部署
（1）bop-front（手动启）对应Jenkins中的bop【先删除main再拉包再修改main文件】
1、打包前先把/apps/bop/bop-front/build/public下的main 文件全部删除：用命令：rm -f main*
2、进入~/apps/bop/bop-front 用mdeploy ls命令查看有哪些包，再用“mdeploy start 包名“获取包
3、打包后把 /apps/bop/bop-front/build/public文件夹下的main文件里的

e:"https://"+window.WEBSITE_HOST+e

改为：

e:"http://"+window.WEBSITE_HOST+e

（因为线上用的是https,测试环境用的是http）

补充：
【先进入到 /apps/bop/bop-front/build/public文件夹，再找到main文件，
用"vi main文件"打开main文件，用"/..."（...为需要修改的内容的部分）查找到要修改的部分，
输入i进入插入模式，修改后按ESC键退出插入模式，输入":wq"保存退出】

拉包后进行如下操作：
1、查看端口：
用 netstat -anp|grep node 命令查看端口
2、kill 3000端口【杀掉这个进程】
kill -9 进程号（上面3000端口对应进程号4712）
3、build目录下起手动起node 程序【再重启这个进程】：
nohup node server.js &

（2）bop-gateway（数据库需要手动配）对应Jenkins中的bop-gateway【先拉包再查看.js文件再手动启】
按上面的方法获取包，打包后确认一下apps/bop/bop-gateway/build/config/config.test.js文件的配置是否是测试环境的，如下面配置

数据库连接（改为测试库）
'MS-DEV': { type: 'mysql', connection: { host: '172.24.2.34', user: 'mobi', password: 'mobisummer2112', database: 'adplatform', port: 3306 } },

然后同样按上面的方法：
kill4567端口
手动起node 程序
nohup node server.js --env test &

【注意front和gateway的手动启命令不同】
