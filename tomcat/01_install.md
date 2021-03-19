###### Tomcat 9.0 설치시 CentOS 7.0 이상버전에서 설치가능 하니, 참고 하세요

### 1. 톰캣 다운로드
  <https://tomcat.apache.org/download-90.cgi>

### 2. JAVA 설치(root 권한)
```console
# yum install java-1.8*
```

### 2. 톰캣설치
```console
[jnet@localhost ~] cd /home/jnet

# wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.44/bin/apache-tomcat-9.0.44.tar.gz
# tar xzvf apache-tomcat-9.0.44.tar.gz
# mv apache-tomcat-9.0.44 tomcat9
```

### 3. tomcat-native 설치

``` console
# yum install gcc apr-1.4* apr-devel openssl*
# cd tomcat/bin
# tar xzvf tomcat-native.tar.gz
# cd tomcat-native-1.2.17-src/native
# configure
# make
# make install
```

### 4. 톰캣 설정
JSVC_OPTS=’-Xms4096m - Xmx4096m -XX:NewSize=2048m -XX:MaxNewSize=2048m’


### [참고사이트]
[CentOS 8에 Apache Tomcat 설치](https://gsggb.blogspot.com/2019/09/centos-7-apache-tomcat-8.html)
