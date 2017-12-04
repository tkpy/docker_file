FROM jon/centos:7.4.1708
MAINTAINER FoxChan
#copy jdk and tomcat into image
ADD ./apache-tomcat-7.0.82.tar.gz /root
ADD ./jdk1.7.0_75 /root/jdk1.7.0_75
#set environment variable
ENV JAVA_HOME /root/jdk1.7.0_75
ENV PATH $JAVA_HOME/bin:$PATH
#define entry point which will be run first when the container starts up
ENTRYPOINT /root/apache-tomcat-7.0.82/bin/startup.sh && tail -F /root/apache-tomcat-7.0.82/logs/catalina.out
