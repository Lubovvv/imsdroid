This wiki page explains how to build Doubango code (ANSI-C/C++) for IMSdroid. Please note that IMSDroid is already distributed with these libraries and you only need to rebuild them for debugging or to disable/enable additional features. You also need to rebuild the libraries to generate **commercial** versions as the default ones are **GPL**. <br />

For the Java code, you just need to import the code into Eclipse.



## Installing Android NDK r10e ##
Download Android NDK r9c into **/home**. This is needed only for the first time. <br />
**IMPORTANT:** You must select the right version from http://developer.android.com/intl/ja/ndk/downloads/index.html depending on your host system. Our host system is **CentOS 64**.
```
yum install p7zip
cd /home
wget http://dl.google.com/android/ndk/android-ndk-r10e-linux-x86_64.bin
7za x android-ndk-r10e-linux-x86_64.bin
```
### Set $NDK ###
You must set the **$NDK** variable:
```
export NDK=/home/android-ndk-r10e
```

## Checking out Doubango code ##
Chekout Doubango v2.0 into **/home**:
```
cd /home
git clone https://github.com/DoubangoTelecom/doubango.git
```

## Building the code ##
The build system uses GNU AutoTools. There are two ways to build the libs: **hard** or **easy**. We highly recommend using the **easy** way.
The first step is to generate the **configure** file:
```
cd /home/doubango
./autogen.sh
```

### The easy way ###
Each script will build the code for all supported architectures.
#### GPL ####
```
./android_build.sh gpl
```
The binaries will be generated into **/home/doubango/android-projects/output/gpl/imsdroid/libs**.
#### Commercial ####
If you have a commercial license:
```
./android_build.sh commercial
```
The binaries will be generated into **/home/doubango/android-projects/output/commercial/imsdroid/libs**.
### The hard way ###
Not really hard but boring :).

To build the code for **ARMv5TE**:
```
./configure --host=arm-linux-androideabi --with-android-cpu=armv5te --prefix=/home/doubango/output/android/armv5te
make && make install
```
for **ARMv7-a without NEON**:
```
./configure --host=arm-linux-androideabi --with-android-cpu=armv7-a --prefix=/home/doubango/output/android/armv7-a
make && make install
```
for **ARMv7-a with NEON**:
```
./configure --host=arm-linux-androideabi --with-android-cpu=armv7-a-neon --prefix=/home/doubango/output/android/armv7-a-neon
make && make install
```
for **ARM64**:
```
./configure --host=aarch64-linux-android --with-android-cpu=arm64 --prefix=/home/doubango/output/android/arm64
make && make install
```
for **x86**:
```
./configure --host=i686-linux-android --with-android-cpu=x86 --prefix=/home/doubango/output/android/x86
make && make install
```
for **x86_64**:
```
./configure --host=x86_64-linux-android --with-android-cpu=x64 --prefix=/home/doubango/output/android/x64
make && make install
```
To get more info about all available options:
```
./configure --help
```

## Known issues ##
#### checking host system type... Invalid configuration `arm-linux-androideabi': system `androideabi' not recognized ####
Your host GCC version is too old and the generated **config.sub** doesn't support android. To fix the issue:
```
cp /home/doubango/new-config.sub /home/doubango/config.sub
```
