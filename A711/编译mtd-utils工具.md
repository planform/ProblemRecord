### 交叉编译MTD-Utils工具 ###

#### 0x01 新建交叉编译文件夹 ####

​	`mkdir crosscompile && cd crosscompile`；

#### 0x02 下载依赖库 ####

* [下载zlib](<http://www.zlib.net/>)；
* [下载lzo](<http://www.oberhumer.com/opensource/lzo/#download>)；
* [下载e2fsprogs](<http://e2fsprogs.sourceforge.net/>)；
* [下载mtd-utils](<http://www.linux-mtd.infradead.org/index.html>)；

#### 0x03 编译安装zlib ####

1. `tar -zxvf zlib-1.2.11.tar.gz`；
2. `cd zlib-1.2.11`；
3. `export CC=aarch64-linux-gnu-gcc`；
4. `./configure --prefix=$PWD/_install`；
5. `make && make install`；

#### 0x04 编译安装lzo ####

1. `tar -zxvf lzo-2.10.tar.gz`；
2. `cd lzo-2.10`；
3. `export CC=aarch64-linux-gnu-gcc`；
4. `./configure --host=arm-linux --enable-static --prefix=$PWD/_install`；
5. `make && make install`；

#### 0x05 编译安装e2fsprogs ####

1. `tar -zxvf e2fsprogs-1.41.14.tar.gz`；
2. `cd e2fsprogs-1.41.14`；
3. `export CC=aarch64-linux-gnu-gcc`；
4. `./configure --prefix=$PWD/_install --enable-static --host=arm-linux` ；
5. `make && make install-libs`；

#### 0x06 编译按章mtd_utils ####

1. `tar -zxvf mtd-utils-2.1.0.tar.gz`；

2. `cd mtd-utils-2.1.0`；

3. ```export ZLIB_CFLAGS=-I/home/lizekun/workspace/crosscompile/build_mtd/zlib-1.2.5
   export CC=aarch64-linux-gnu-gcc
   export ZLIB_LIBS=-L/home/lizekun/workspace/crosscompile/build_mtd/zlib-1.2.5/_install/lib
   export LZO_CFLAGS=-I/home/lizekun/workspace/crosscompile/build_mtd/lzo-2.03/_install/include
   export LZO_LIBS=-L/home/lizekun/workspace/crosscompile/build_mtd/lzo-2.03/_install/lib
   export UUID_CFLAGS=-I/home/lizekun/workspace/crosscompile/build_mtd/e2fsprogs-1.41.14/_install/include/uuid
   export UUID_LIBS=-L/home/lizekun/workspace/crosscompile/build_mtd/e2fsprogs-1.41.14/_install/lib
   export LDFLAGS="$ZLIB_LIBS $LZO_LIBS $UUID_LIBS --static -luuid -lz"
   export CFLAGS="-O2 -g --static $ZLIB_CFLAGS $LZO_CFLAGS $UUID_CFLAGS"；
   ```

4. ` ./configure --host=arm-linux CC=aarch64-linux-gnu-gcc --prefix=$PWD/install_dir --without-ubifs --enable-static` ；

5. `make CROSS=aarch64-linux-gnu- WITHOUT_XATTR=1`；

6. `make install`；



