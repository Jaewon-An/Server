
### 1. Apache yum 설치
```console
# yum list installed | grep httpd
# yum -y install httpd httpd-devel libtool
# systemctl enable httpd
```

### 2. mod_jk 설치
* 다운로드 페이지 : [바로가기](http://tomcat.apache.org/download-connectors.cgi)
* modules 디렉토리 아래 설치 됨
```console
# yum -y install gcc gcc-c++
# yum -y install httpd-devel
# cd /usr/local/src
# wget https://downloads.apache.org/tomcat/tomcat-connectors/jk/tomcat-connectors-1.2.48-src.tar.gz
# tar xvf tomcat-connectors-1.2.48-src.tar.gz
# cd tomcat-connectors-1.2.48-src/native
# ./configure --with-apxs=/usr/bin/apxs
# make && make instal
```



