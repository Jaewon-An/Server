
### 1. Apache yum 설치
```console
# yum list installed | grep httpd
# yum -y install httpd httpd-devel libtool
# systemctl enable httpd
```

### 2. mod_jk 설치
* 다운로드 페이지 : [바로가기](http://tomcat.apache.org/download-connectors.cgi)
* /etc/httpd/modules 디렉토리 아래 설치 됨

```console
# yum -y install gcc gcc-c++
# yum -y install httpd-devel
# cd /usr/local/src
# wget https://downloads.apache.org/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.48-src.tar.gz
# tar xvf tomcat-connectors-1.2.48-src.tar.gz
# cd tomcat-connectors-1.2.48-src/native
# ./configure --with-apxs=/usr/bin/apxs
# make && make install
```
### 3. jk 모듈 설정
```configure
LoadModule jk_module modules/mod_jk.so

<IfModule mod_jk.c>
  JkWorkersFile conf/workers.properties
  #JkShmFile 내용 반드시 넣어줄 것 - Selinux 보안때문에 공유 메모리파일 위치 반드시 명시해야함
  JkShmFile run/mod_jk.shm
  JkLogFile logs/mod_jk.log
  JkLogLevel info
  JkLogStampFormat "[%y %m %d %H:%M:%S] "
</IfModule>
```
### 4. workers.properties 파일생성
```configure
worker.list=ajp13
worker.ajp13.port=8009
worker.ajp13.host=localhost
worker.ajp13.type=ajp13
worker.ajp13.lbfactor=1
```
### 5. VirtualHost 설정
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
