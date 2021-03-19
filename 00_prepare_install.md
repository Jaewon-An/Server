### 1. 사용자 추가
```console
[root@localhost ~]# useradd jnet
[root@localhost ~]# passwd jnet
```
### 2. 관련 rpm 설치
```console
[root@localhost ~]# yum gcc gcc-c++ make automake autoconf redhat-rpm-config openssl* libtool
```
### 3. JAVA 설치
```console
[root@localhost ~]# yum install java-1.8*
[root@localhost ~]# vi /etc/profile
export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
```
