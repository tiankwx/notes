# FreeSWITCH Install

## AnolisOS8.2 编译安装
```
# Linux AnolisOS82 4.18.0-193.60.2.an8_2.x86_64 #1 SMP Tue Aug 17 16:16:36 CST 2021 x86_64 x86_64 x86_64 GNU/Linux
# FreeSWITCH Version 1.10.8-dev+git~20220328T145617Z~8f9f5c1c3e~64bit (git 8f9f5c1 2022-03-28 14:56:17Z 64bit)

# yum install -y epel-release
yum groupinstall -y "Development Tools"
yum install -y speex-devel speexdsp-devel libcurl-devel libtiff-devel libjpeg-devel lua-devel libuuid-devel cmake ldns-devel libidn-devel unbound-devel wget libatomic openssl-devel libedit* opus-devel libsndfile-devel sqlite-devel
dnf install -y libpq-devel yasm -y
yum -y update && yum -y upgrade

# 安装过程中根据出错信息安装下面的依赖

cd /usr/local/src
git clone --depth=1 https://gitee.com/nwaycn/freeswitch.git
cd /usr/local/src/freeswitch
./bootstrap.sh -j
./configure
make
make install
make cd-sounds-install
make cd-moh-install

# 完成安装之后，设置符号链接：
ln -sf /usr/local/freeswitch/bin/freeswitch /usr/local/bin/
ln -sf /usr/local/freeswitch/bin/fs_cli /usr/local/bin/

############################################################################################################################################
# checking for spandsp >= 3.0... configure: error: no usable spandsp; please install spandsp3 devel package or equivalent
cd /usr/local/src && git clone https://gitee.com/FreeSWITCHs/spandsp.git && cd spandsp
./bootstrap.sh -j 
./configure
make 
make install
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:${PKG_CONFIG_PATH}
ldconfig
############################################################################################################################################
# checking for sofia-sip-ua >= 1.13.6... configure: error: no usable sofia-sip; please install sofia-sip-ua devel package or equivalent
cd /usr/local/src && git clone --depth=1 https://gitee.com/FreeSWITCHs/sofia-sip.git && cd sofia-sip
sh autogen.sh
./configure
make
make install
############################################################################################################################################
cd /usr/local/src && wget https://udomain.dl.sourceforge.net/project/pcre/pcre/8.45/pcre-8.45.zip && unzip pcre-8.45.zip && cd pcre-8.45
./configure 
make && make install
############################################################################################################################################
# checking for libks >= 1.1.0... configure: error: You need to either install libks or disable mod_verto in modules.conf
cd /usr/local/src && git clone https://gitee.com/FreeSWITCHs/libks.git && cd libks
cmake .
make
make install
#这个比较坑爹，不然还是还找不到libks模块
cp /usr/lib/pkgconfig/libks.pc /usr/lib64/pkgconfig/
############################################################################################################################################
# checking for signalwire_client >= 1.0.0... configure: error: You need to either install signalwire-client-c or disable mod_signalwire in modules.conf
cd /usr/local/src && git clone https://gitee.com/FreeSWITCHs/signalwire-c.git && cd signalwire-c
cmake .
make && make install
export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig:/usr/local/lib64/pkgconfig:${PKG_CONFIG_PATH}
ldconfig
############################################################################################################################################
# Neither yasm nor nasm have been found. See the prerequisites section in the README for more info.
curl http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz >yasm.tar.gz
tar xzvf yasm.tar.gz
cd yasm-1.2.0
./configure
make
make install
############################################################################################################################################

```
---
## AnolisOS8.2 编译安装最新版本
### Linux AnolisOS82 4.18.0-193.70.1.an8_2.x86_64 #1 SMP Wed Dec 1 17:52:32 CST 2021 x86_64 x86_64 x86_64 GNU/Linux
### FreeSWITCH Version 1.10.8-dev+git~20220328T145617Z~8f9f5c1c3e~64bit (git 8f9f5c1 2022-03-28 14:56:17Z 64bit)
```
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
LANG=en_US.UTF-8
yum install -y epel-release
rpm -ivh http://repo.okay.com.mx/centos/8/x86_64/release/okay-release-1-5.el8.noarch.rpm
dnf localinstall -y https://pkgs.dyn.su/el8/base/x86_64/raven-release-1.0-2.el8.noarch.rpm
yum -y update && yum -y upgrade

yum install -y git alsa-lib-devel autoconf automake bison broadvoice-devel bzip2 curl-devel libdb4-devel e2fsprogs-devel erlang flite-devel g722_1-devel gcc-c++ gdbm-devel gnutls-devel ilbc2-devel ldns-devel libcodec2-devel libidn-devel libjpeg-devel libmemcached-devel libogg-devel libsilk-devel libsndfile-devel libtheora-devel libtiff-devel libtool libuuid-devel libvorbis-devel libxml2-devel lua-devel lzo-devel mongo-c-driver-devel ncurses-devel net-snmp-devel openssl-devel opus-devel pcre-devel perl perl-ExtUtils-Embed pkgconfig portaudio-devel postgresql-devel soundtouch-devel speex-devel unbound-devel unixODBC-devel wget which yasm zlib-devel libshout-devel libmpg123-devel lame-devel rpm-build libX11-devel sqlite* libcurl-devel speexdsp-devel libedit-devel 

dnf install -y spandsp-devel sofia-sip-devel libks-devel signalwire-client-c-devel
dnf install libav-devel -y

cd /usr/local/src && git clone git clone https://gitee.com/FreeSWITCHs/x264.git && cd x264
./configure --enable-shared --enable-static --enable-debug --disable-asm
make && make install

cd /usr/local/src
git clone https://gitee.com/nwaycn/freeswitch.git
cd /usr/local/src/freeswitch
./bootstrap.sh -j
./configure
make
make install
make cd-sounds-install
make cd-moh-install

# 完成安装之后，设置符号链接：
ln -sf /usr/local/freeswitch/bin/freeswitch /usr/local/bin/
ln -sf /usr/local/freeswitch/bin/fs_cli /usr/local/bin/

```
---
## AnolisOS8.2 第三方源包安装
### Linux AnolisOS82 4.18.0-193.70.1.an8_2.x86_64 #1 SMP Wed Dec 1 17:52:32 CST 2021 x86_64 x86_64 x86_64 GNU/Linux
### FreeSWITCH Version 1.10.7-release~64bit (-release 64bit)
```
#!/bin/bash
PATH=/bin:/sbin:/usr/bin:/usr/sbin:/usr/local/bin:/usr/local/sbin:~/bin
export PATH
LANG=en_US.UTF-8
yum install -y epel-release wget zip unzip
rpm -ivh http://repo.okay.com.mx/centos/8/x86_64/release/okay-release-1-5.el8.noarch.rpm
yum -y update && yum -y upgrade
dnf install -y freeswitch
cd ~
rm -rf /etc/freeswitch
git clone https://gitee.com/FreeSWITCHs/freeswitch-minimal-conf.git /etc/freeswitch/

```
### 安装的依赖
```
alsa-lib-1.2.1.2-3.el8.x86_64
flac-libs-1.3.2-9.el8.x86_64
freeswitch-1:1.10.7-1.el8.x86_64
freeswitch-cli-1:1.10.7-1.el8.x86_64
gperftools-libs-1:2.7-9.el8.x86_64
gsm-1.0.17-5.el8.x86_64
libfvad-1.1.0-1.el8.x86_64
libgumbo1-0.10.1-1.el8.x86_64
libjwt-1.13.1-1.el8.x86_64
libks-1.7.0-1.el8.x86_64
libogg-2:1.3.2-10.el8.x86_64
libpq-12.7-1.el8.x86_64
libsndfile-1.0.28-10.el8.x86_64
libstirshaken-0.0.0-0.4.el8.x86_64
libtpl-1.5-9.el8.x86_64
libunwind-1.3.1-3.el8.x86_64
libvorbis-1:1.3.6-2.el8.x86_64
python2-2.7.17-1.0.1.module+el8.2.0+10127+a910aa2a.x86_64
python2-libs-2.7.17-1.0.1.module+el8.2.0+10127+a910aa2a.x86_64
python2-pip-9.0.3-16.module+el8.2.0+10127+a910aa2a.noarch
python2-pip-wheel-9.0.3-16.module+el8.2.0+10127+a910aa2a.noarch
python2-setuptools-39.0.1-11.0.1.module+el8.2.0+10127+a910aa2a.noarch
python2-setuptools-wheel-39.0.1-11.0.1.module+el8.2.0+10127+a910aa2a.noarch
sofia-sip-1.13.6-1.el8.x86_64
spandsp-3.0.0-1.el8.x86_64
speex-1.2.0-1.el8.x86_64
speexdsp-1.2-0.13.rc3.el8.x86_64
unixODBC-2.3.7-1.el8.x86_64
```
---
## AnolisOS7.9 Install
```
# 使用自建Repo安装
yum install -y http://qiniu.kodo.nationality.test.com/repo/yum/centos-release/freeswitch-release-repo-0-1.noarch.rpm epel-release
rm -rf /etc/yum.repos.d/freeswitch-release.repo
wget -O /etc/yum.repos.d/freeswitch-release.repo http://qiniu.kodo.nationality.test.com/repo/yum/centos-release/freeswitch-release.repo
yum -y update && yum -y upgrade
yum install -y freeswitch-config-vanilla freeswitch-lang-* freeswitch-sounds-*
systemctl enable freeswitch
```
---
## 官方安装方法
### 要注册帐号，获取Token
### [安装说明](https://freeswitch.org/confluence/display/FREESWITCH/CentOS+7+and+RHEL+7)
```
echo "signalwire" > /etc/yum/vars/signalwireusername
echo "TOKEN" > /etc/yum/vars/signalwiretoken
yum install -y https://$(< /etc/yum/vars/signalwireusername):$(< /etc/yum/vars/signalwiretoken)@freeswitch.signalwire.com/repo/yum/centos-release/freeswitch-release-repo-0-1.noarch.rpm epel-release
yum install -y freeswitch-config-vanilla freeswitch-lang-* freeswitch-sounds-*
systemctl enable freeswitch
```
