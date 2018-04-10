# Test-environment-deployment
# 权限系统测试环境部署

权限系统的【am-auth-front】和【am-auth-gateway】在Jenkins上打包（node撰写的代码，需要手动启）
权限系统的后端读写权限在打包平台上打包（Java撰写，可直接mdeploy start获取包，不需要手动启）

一、am-auth-front：
先mdeploy获取包（具体包名可以在Jenkins上打开相应模块，左侧寻找相应的修改项下拉选择Console Output控制台输出，拉到最底查看包名）
获取包后，进入build文件夹，vi server.js文件对相应需要修改的端口和IP地址进行修改 :wq保存退出
netstat -anp|grep node查看端口，kill掉3030端口，并在build文件夹下nohup node server.js &手动启进程
用tail -50f xxx.out 命令查看日志

二、am-auth-gateway
同样的获取包方法
获取包后，进入build/config/config.test.js文件修改端口为6789，将IP地址中的26改为34，:wq保存退出
kill6789端口再手动启
