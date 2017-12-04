# wordpress与cent-tomcat搭建

## 一.创建一个xdl/cent-tomcat:7.0.82镜像 
### 1.xdl可以自定义,记得Dockerfile里也要改.指定版本为7.0.82

    docker build  -t xdl/cent-tomcat:7.0.82 .
### 2.启动tomcat
    
运行xdl/cent-tomcat:7.0.82 访问端口为:8090,NAMES为tomcat  

    docker run -d -p 8090:8080 --name tomcat xdl/cent-tomcat:7.0.82
用docker ps -a命令查看状态
```
[root@centos7 ~/docker_file/tomcat7]# docker ps -a
CONTAINER ID        IMAGE                    COMMAND                  CREATED              STATUS              PORTS                            NAMES
2d97d0315118        jon/cent-tomcat:7.0.82   "/bin/sh -c '/root..."   About a minute ago   Up About a minute   22/tcp, 0.0.0.0:8090->8080/tcp   tomcat
```
用浏览器访问8090,有下面页面就表示成功了
![image.png](http://upload-images.jianshu.io/upload_images/9298220-6dcb7e81deaa79e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##  二.创建php-fpm5.4 ,msyql5.5 ,wordpress4.2
#### 1.创建php-fpm指定版本为php-fpm5.4

    docker build -t jon/php-fpm:5.4 .
#### 2.创建mysql5.5

    docker build -t jon/mysql:5.5 .
运行mysql

	docker run -d -p 3306:3306 --name dbserver jon/mysql:5.5
#### 3.创建wordpress:4.2

	docker build -t xdl/wordpress:4.2 .
#### 4.进入mysql5.5 给mysql 设置访问用户和密码

进入mysql  如果不知道mysql 的名字用docker ps查看
    
	docker exec -it dbserver /bin/bash
设置密码

格式：
grant允许操作on库名(这里的.为所有库) to 账号@来源(%是所有ip可以访问) identified by '密码'; 
```

	mysql> grant all on *.* to zhangsan@'%' identified by '123'; 
	Query OK, 0 rows affected (0.01 sec)
	mysql> flush privileges;  # 生效
	Query OK, 0 rows affected (0.00 sec)

	密码不能为特殊字符
```



### 5.运行镜像wordpress 镜像ip为172.17.0.5 mysql的用户为admin密码为xxx
```
docker run -d -p 80:80 --name wordpress -e WORDPRESS_DB_HOST=172.17.0.5 -e WORDPRESS_DB_USER=admin -e WORDPRESS_DB_PASSWORD=xxx jon/wordpress:4.2
```

用浏览器访问你的服务器看到下图就成功了
![image.png](http://upload-images.jianshu.io/upload_images/9298220-26a34997ad6375ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)




ps:如果有错用docker logs 容器id   看日志
