
### 1. Apache yum 설치
```console
[root@localhost ~]# yum list installed | grep httpd
[root@localhost ~]# yum install httpd httpd-devel libtool
[root@localhost ~]# systemctl enable httpd
[root@localhost ~]# systemctl start httpd
[root@localhost ~]# netstat -nltp | grep 80
```
### 2. mod_jk 설치
##### 다운로드 페이지 : [바로가기](http://tomcat.apache.org/download-connectors.cgi)
##### /etc/httpd/modules(/usr/lib64/httpd/modules) 디렉토리 아래 설치 됨

```console
[root@localhost ~]# yum install gcc gcc-c++
[root@localhost ~]# cd /usr/local/src
[root@localhost src]# wget https://downloads.apache.org/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.48-src.tar.gz
[root@localhost src]# tar xvf tomcat-connectors-1.2.48-src.tar.gz
[root@localhost src]# cd tomcat-connectors-1.2.48-src/native
[root@localhost native]# ./configure --with-apxs=/usr/bin/apxs
[root@localhost native]# make && make install
[root@localhost native]# ls -al /usr/lib64/httpd/modules | grep mod_jk*
```
### 3. workers.properties 파일생성
```console
[root@localhost ~]# cd /etc/httpd/conf
[root@localhost conf]# vi workers.properties
```
workers.properties 내용
```configure
worker.list=ajp13
worker.ajp13.port=8009
worker.ajp13.host=localhost
worker.ajp13.type=ajp13
worker.ajp13.lbfactor=1
```

### 3. jk 모듈 설정
```console
[root@localhost ~]# cd /etc/httpd/conf.d
[root@localhost conf.d]# vi mod_jk.conf
```
00-jk.conf 파일 내용
```configure
LoadModule jk_module modules/mod_jk.so

<IfModule mod_jk.c>
  JkWorkersFile conf.d/workers.properties
  #JkShmFile 내용 반드시 넣어줄 것 - Selinux 보안때문에 공유 메모리파일 위치 반드시 명시해야함
  JkShmFile run/mod_jk.shm
  JkLogFile logs/mod_jk.log
  JkLogLevel info
  JkLogStampFormat "[%y %m %d %H:%M:%S] "
</IfModule>
```
### 5. VirtualHost 설정
```console
[root@localhost ~]# cd /etc/httpd/conf.d
[root@localhost conf.d]# vi vhost.conf
```
vhost.conf 파일 내용
```configure
<VirtualHost *:80>
  ServerName www.yesjnet.com
  ServerAdmin webmaster@yesjnet.com
  DocumentRoot "/home/jnet/webroot/homepage"
  CustomLog logs/homepage.access.log common
  ErrorLog logs/homepage.error.log

  JkMount / ajp13
  JkMount /* ajp13

  #http 호출시 https로 리다이렉트
  Redirect permanent / https://www.yesjnet.com/
</VirtualHost>
```
### 6. SameSite none
```configure
<VirtualHost _default_:443>
  .
  .
  .
  Header edit Set-Cookie ^(.*)$ $1;HttpOnly;Secure;SameSite=None
</VirtualHost>
```
