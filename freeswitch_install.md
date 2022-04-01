# FreeSWITCH Install

## AnolisOS8.2 Install
```
# Linux AnolisOS82 4.18.0-193.60.2.an8_2.x86_64 #1 SMP Tue Aug 17 16:16:36 CST 2021 x86_64 x86_64 x86_64 GNU/Linux
# FreeSWITCH Version 1.10.8-dev+git~20220328T145617Z~8f9f5c1c3e~64bit (git 8f9f5c1 2022-03-28 14:56:17Z 64bit)

yum install -y epel-release
yum install https://download1.rpmfusion.org/free/el/rpmfusion-free-release-8.noarch.rpm
yum groupinstall "Development Tools"
yum install libuuid-devel libatomic openssl-devel libedit*
yum install speex speex-devel speexdsp-devel libcurl-devel libtiff-devel libjpeg-devel lua-devel libuuid-devel sqlite* cmake ldns-devel libidn-devel unbound-devel ffmpeg ffmpeg-devel wget
yum -y install opus-devel libsndfile-devel
dnf install libpq-devel

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
cd /usr/local/src && git clone https://gitee.com/FreeSWITCHs/yasm.git && cd yasm && ./autogen.sh && make && make install

curl http://www.tortall.net/projects/yasm/releases/yasm-1.2.0.tar.gz >yasm.tar.gz
tar xzvf yasm.tar.gz
cd yasm-1.2.0
./configure
make
make install
############################################################################################################################################

```
