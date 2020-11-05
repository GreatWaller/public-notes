## vs code C++ 开发环境配置

C/C++ Extension Configurations

Include path

在命令行输入 g++ -v -E -x c++ -

```bash
$ g++ -v -E -x c++ -
Using built-in specs.
COLLECT_GCC=g++
Target: aarch64-linux-gnu
Configured with: ../src/configure -v --with-pkgversion='Ubuntu/Linaro 7.5.0-3ubuntu1~18.04' --with-bugurl=file:///usr/share/doc/gcc-7/README.Bugs --enable-languages=c,ada,c++,go,d,fortran,objc,obj-c++ --prefix=/usr --with-gcc-major-version-only --program-suffix=-7 --program-prefix=aarch64-linux-gnu- --enable-shared --enable-linker-build-id --libexecdir=/usr/lib --without-included-gettext --enable-threads=posix --libdir=/usr/lib --enable-nls --enable-bootstrap --enable-clocale=gnu --enable-libstdcxx-debug --enable-libstdcxx-time=yes --with-default-libstdcxx-abi=new --enable-gnu-unique-object --disable-libquadmath --disable-libquadmath-support --enable-plugin --enable-default-pie --with-system-zlib --enable-multiarch --enable-fix-cortex-a53-843419 --disable-werror --enable-checking=release --build=aarch64-linux-gnu --host=aarch64-linux-gnu --target=aarch64-linux-gnu
Thread model: posix
gcc version 7.5.0 (Ubuntu/Linaro 7.5.0-3ubuntu1~18.04) 
COLLECT_GCC_OPTIONS='-v' '-E' '-shared-libgcc' '-mlittle-endian' '-mabi=lp64'
 /usr/lib/gcc/aarch64-linux-gnu/7/cc1plus -E -quiet -v -imultiarch aarch64-linux-gnu -D_GNU_SOURCE - -mlittle-endian -mabi=lp64 -fstack-protector-strong -Wformat -Wformat-security
ignoring duplicate directory "/usr/include/aarch64-linux-gnu/c++/7"
ignoring nonexistent directory "/usr/local/include/aarch64-linux-gnu"
ignoring nonexistent directory "/usr/lib/gcc/aarch64-linux-gnu/7/../../../../aarch64-linux-gnu/include"
#include "..." search starts here:
#include <...> search starts here:
 /usr/include/c++/7
 /usr/include/aarch64-linux-gnu/c++/7
 /usr/include/c++/7/backward
 /usr/lib/gcc/aarch64-linux-gnu/7/include
 /usr/local/include
 /usr/lib/gcc/aarch64-linux-gnu/7/include-fixed
 /usr/include/aarch64-linux-gnu
 /usr/include
End of search list.
```

添加

/usr/include/c++/7
/usr/include/aarch64-linux-gnu/c++/7
/usr/include/c++/7/backward
/usr/lib/gcc/aarch64-linux-gnu/7/include
/usr/local/include
/usr/lib/gcc/aarch64-linux-gnu/7/include-fixed
/usr/include/aarch64-linux-gnu
/usr/include
/opt/ros/melodic/include
/usr/include/pcl-1.8
/usr/include/eigen3
/usr/include/opencv4





或

/usr/include/**
/opt/ros/melodic/include