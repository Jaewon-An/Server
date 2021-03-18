### 1. Apache yum 설치
```console
# yum list installed | grep httpd
# yum -y install httpd httpd-devel libtool
# systemctl enable httpd
```

### 2. mod_jk 설치
```console
# yum -y install gcc gcc-c++
# yum -y install httpd-devel
./configure --with-apxs=/usr/bin/apxs

