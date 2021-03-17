# deb
## Ubuntu生成deb包

```shell
mkdir -p /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN
mkdir -p /home/chaofei/deb/hellodeb-0.0.1-1/usr/local/src/hellodeb-0.0.1-1
cp -r /home/chaofei/hellodeb-0.0.1-1.x86_64/* /home/chaofei/deb/hellodeb-0.0.1-1/usr/local/src/hellodeb-0.0.1-1/
```
### 版本信息
```shell
tee /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/control << EOF
Package: hellodeb
Version: 0.0.1-1
Architecture: all
Maintainer: Fei Chao
Installed-Size: 324
Recommends:
Suggests:
Section: devel
Priority: optional
Multi-Arch: foreign
Description: this is a hellodeb test
EOF

```
### 安装前执行
```shell
tee /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postinst  << EOF
mkdir -p ${HOME}/bin
echo mkdir -p ${HOME}/bin >>/tmp/chaofeirpm.log
cp /usr/local/src/hellodeb-0.0.1-1/test-main ${HOME}/bin/test-main
chmod a+x ${HOME}/bin/test-main
cp /usr/local/src/hellodeb-0.0.1-1/test-main /usr/bin/test-main
chmod a+x /usr/bin/test-main
echo ${HOME}/bin/test-main
${HOME}/bin/test-main
echo ${HOME}/bin/test-main >>/tmp/chaofeirpm.log
EOF

```

### 卸载后执行
```shell
tee /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postrm << EOF
rm -f ${HOME}/bin/test-main
rm -f /usr/bin/test-main
rm -f /tmp/chaofeirpm.log
EOF

```

### 生成hellodeb包
```shell
chmod 665 /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/control
chmod 775 /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postinst
chmod 775 /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postrm 
cd /home/chaofei/deb/
dpkg -b hellodeb-0.0.1-1 hellodeb-0.0.1-1.deb
```
```shell
chaofei@ubuntu:~/deb$ chmod 775 /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postinst
chaofei@ubuntu:~/deb$ chmod 775 /home/chaofei/deb/hellodeb-0.0.1-1/DEBIAN/postrm 
chaofei@ubuntu:~/deb$ cd /home/chaofei/deb/
chaofei@ubuntu:~/deb$ dpkg -b hellodeb-0.0.1-1 hellodeb-0.0.1-1.deb
dpkg-deb: building package `hellodeb' in `hellodeb-0.0.1-1.deb'.

```

### 安装hellodeb包
```shell
cd /home/chaofei/deb/
sudo dpkg -i hellodeb-0.0.1-1.deb
```
```shell
chaofei@ubuntu:~/deb$ sudo dpkg -i hellodeb-0.0.1-1.deb
[sudo] password for chaofei: 
Selecting previously unselected package hellodeb.
(Reading database ... 61809 files and directories currently installed.)
Preparing to unpack hellodeb-0.0.1-1.deb ...
Unpacking hellodeb (0.0.1-1) ...
Setting up hellodeb (0.0.1-1) ...
/home/chaofei/bin/test-main
max(2, 8) = 8
change begin a=2 b=8
change end a=8 b=2
Hello, I'm chaofei
This is a test of rpm build
```

### 查看hellodeb包
```shell
chaofei@ubuntu:~$ dpkg -l | grep -i hellodeb
ii  hellodeb                            1.0                           all          this is a hellodeb test
```
```shell
chaofei@ubuntu:~/deb$ test-main
max(2, 8) = 8
change begin a=2 b=8
change end a=8 b=2
Hello, I'm chaofei
This is a test of rpm build
chaofei@ubuntu:~/deb$ ls /usr/local/src/hellodeb-0.0.1-1/
configure  Makefile-0.0.1  Makefile-0.0.3  Makefile-root        Makefile-v0.0.1  Makefile-v0.0.3  Makefile-v0.0.5  max.c  max.o      swap.c  swap.o     test-main.c  test-patch.txt
Makefile   Makefile-0.0.2  Makefile-0.0.4  Makefile-simpleuser  Makefile-v0.0.2  Makefile-v0.0.4  Makefile-v0.0.6  max.h  README.md  swap.h  test-main  test-main.o
chaofei@ubuntu:~/deb$ ls /tmp/chaofeirpm.log 
/tmp/chaofeirpm.log
chaofei@ubuntu:~/deb$ dpkg -l | grep hellodeb
ii  hellodeb                            0.0.1-1                       all          this is a hellodeb test
chaofei@ubuntu:~/deb$ 
```

### 卸载hellodeb包
```shell
sudo dpkg -r hellodeb
sudo dpkg --purge hellodeb
```
