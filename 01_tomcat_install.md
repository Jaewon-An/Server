### 1. Tomcat 설치(jnet 계정 사전생성필요)
###### Tomcat 9.0 설치시 CentOS 7.0 이상버전에서 설치가능 하니, 참고 하세요
```console
[root@localhost ~]# cd /home/jnet
[root@localhost jnet]# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.44/bin/apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# tar xzvf apache-tomcat-9.0.44.tar.gz
[root@localhost jnet]# mv apache-tomcat-9.0.44 tomcat9
```

### 2. Tomcat 실행 옵션 파일 생성
 setenv.sh 파일생성
```console
[root@localhost ~]# cd /home/jnet/tomcat9/bin
[root@localhost bin]#  vi setenv.sh
[root@localhost bin]## chmod a+x setenv.sh
```

setenv.sh 파일 내용
```
#!/bin/bash
CATALINA_OPTS="$CATALINA_OPTS -server -Dfile.encoding=UTF8 -Duser.timezone=GMT+9"
CATALINA_OPTS="$CATALINA_OPTS -Xms4096m -Xmx4096m"
```

### 3. tomcat-native 설치
tomcat-native 설치
```console
[root@localhost ~]# yum install gcc make redhat-rpm-config apr* openssl*
[root@localhost ~]# cd /home/jnet/tomcat9/bin
[root@localhost bin]# tar xzvf tomcat-native.tar.gz
[root@localhost bin]#  cd tomcat-native-1.2.26-src/native
[root@localhost native]#  ./configure
[root@localhost native]# make && make install
```
etc.profile 수정
```console
[root@localhost native]## vi /etc/profile
```
etc.profile 추가 내용
```
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/apr/lib
```

### 4. 소유자 설정 및 테스트
작업 완료 후 소유자 jnet 로 변경
```console
[root@localhost ~]# cd /home/jnet/
[root@localhost ~]# chown jnet:jnet -R /home/jnet/tomcat9
[root@localhost ~]# su - jnet
[jnet@localhost ~]$ cd /home/jnet/tomcat9/bin
[jnet@localhost bin]$ ./startup
[jnet@localhost bin]$ curl http://localhost:8080
[jnet@localhost bin]$ tail -100f /home/jnet/tomcat9/logs/catalina.out
```
톰캣 종료
```console
[jnet@localhost ~]$ cd /home/jnet/tomcat9/bin
[jnet@localhost bin]$ ./shutdown.sh
```
### 5. 자동실행 설정
tomcat.service 파일 생성
```console
[root@localhost ~]# cd /usr/lib/systemd/system
[root@localhost system]# vi tomcat.service
```
tomcat.service 내용
```
[Unit]
Description=Apache Tomcat Web Application Container
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/home/jnet/tomcat9/bin/startup.sh
ExecStop=/bin/kill -15 $MAINPID

User=jnet
Group=jnet
UMask=0007
RestartSec=10
Restart=always

[Install]
WantedBy=multi-user.target
```
tomcat.service 등록 및 실행 테스트
```console
[root@localhost system]# systemctl enable tomcat.service
[root@localhost system]# systemctl start tomcat
[root@localhost system]# systemctl stop tomcat
```
### 6. AJP13 설정
server.xml 수정
```console
[root@localhost ~]# su - jnet
[jnet@localhost ~]$ vi ~/tomcat9/conf/server.xml
```
server.xml ajp13 컨넥터 주석 해제 및 수정
```xml
<Connector protocol="AJP/1.3"
           address="0.0.0.0"
           port="8009"
           redirectPort="8443"
           secretRequired="false" URIEncoding="UTF-8"/>
```

### [아파치설치 및 연동 바로가기](https://github.com/Jaewon-An/Server/blob/main/02_apache_install.md)
