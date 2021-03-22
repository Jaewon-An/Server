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
### 4.  SELINUX 끄기
* 현재 상태에서만 보안 설정에 대한 활성화 및 비활성화를 설정 할 수 있다.시스템 재부팅 후 원래 상태로 돌아 온다. (휘발성)
sestatus: 상태확인, 0: 비활성화, 1`:활성화
```console
[root@localhost ~]# sestatus
[root@localhost ~]# setenforce 0
[root@localhost ~]# setenforce 1
```
* 설정 파일 변경
SELINUX=enforcing (사용함)
SELINUX=perimssive (보안경고만 사용)
SELINUX=disabled (사용안함)
```console
[root@localhost ~]# vi /etc/sysconfig/selinu
SELINUX=disabled
```


### > [톰캣설치 바로가기](https://github.com/Jaewon-An/Server/blob/main/01_tomcat_install.md)
