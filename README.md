
一、使用方法
外网访问地址:XXX:12048/jenkins
用户名:XXXXXX
密码:XXXXXX
登录系统后，选择待发布的系统点击"立即构建"，系统会从代码库获取最新代码自动发布


二.搭建流程
1.官网下载jekins.war包
2. 启动jekins ( java -jar jenkins.war --httpPort=12048)
3.访问 jekins

4.创建待发布的项目，

   4.1配置从svn获取的地址

4.2思路
1.jenkin从代码库获取最新代码
2.编译前杀死tomcat进程，（shell脚本，这里写的killport,根据端口号去杀死）
3. 通过maven 的打包命令，将svn上的最新代码打包到jenkins的目录下
4.将jenkins里面的发布目录，复制到tomcat目录下（具体shell脚本在下面）
5.重新启动tomcat（具体shell脚本在下面）
截图如下:



上图中build的-p web中的web是指下面的父模块中的profiles里面的配置的web，即要编译的模块


killport脚本(killport.sh)
#!/bin/bash
name=$(lsof -i:8080|tail -1|awk '"$1"!=""{print $2}')
if [ -z $name ]
then  
    exit 0  
else 
	kill -9 $name
exit 0
fi

复制发布文件脚本
rm -rf /usr/local/apache-tomcat-8.5.4/webapps/yilingDemo-web
cp -rf /root/.jenkins/workspace/yilingDemo-web/yilingDemo-web/target/yilingDemo-web /usr/local/apache-tomcat-8.5.4/webapps/yilingDemo-web
启动tomcat脚本
BUILD_ID=dontKillMeImparen
sh /usr/local/apache-tomcat-8.5.4/bin/startup.sh
