prismcurl
=========
Use this as a curl submodule with built-in OpenSSL support. This submodule uses Prism's OpenSSL submodule in order to draw in pre-compiled binaries for our supported platforms. The binaries in this submodule should, at all times, match the version of the source.

Source Files
------------
Source files are located in the src directory. If you update the source files to a later build of CURL it is your responsibility to update the binaries for each platform along with it.

Using the Binaries
------------------
Binaries and platform specific files are located under the build/<platform> directories. For insantce, if you wanted to build against this submodule for windows, you would add build/windows/include to your include path and build/windows/lib to your library path.

Adding Another Platform
-----------------------
If you wish to add another platform simply use the configure script in the source directory to configure CURL for your target platform with the prefix pointing to build/<platform>. Once everything is configured to your satisfaction just make and make install and everything should show up in build/<platform> as expected. If you need to cross-compile, things get a bit tricky, for example, this is what is required in order to build for ISD:
<pre><code>cd src
export CROSS="arm-linux-gnueabihf"
export CPPFLAGS="-I/home/vagrant/edge/3rdParty/openssl/build/isd/include -isystem ${ISD_TOP_DIR}/${CROSS}/include -mfloat-abi=hard -mfpu=neon -mcpu=cortex-a9"
export CFLAGS="-isystem ${ISD_TOP_DIR}/${CROSS}/include -mfloat-abi=hard -mfpu=neon -mcpu=cortex-a9"
export LDFLAGS="-z muldefs -L/home/vagrant/edge/3rdParty/openssl/build/isd/lib -L${ISD_TOP_DIR}/lib -L${ISD_TOP_DIR}/${CROSS}/lib -Wl,-rpath-link=${ISD_TOP_DIR}/lib:${ISD_TOP_DIR}/${CROSS}/lib -lrt"
export AR="arm-linux-gnueabihf-ar"
export AS=arm-linux-gnueabihf-as
export LD=arm-linux-gnueabihf-ld
export RANLIB=arm-linux-gnueabihf-ranlib
export CC=arm-linux-gnueabihf-gcc
export NM=arm-linux-gnueabihf-nm
export LIBS="-lrt"
./configure --prefix=/home/vagrant/curl --target=${CROSS} --host=${CROSS} --build=i686-pc-linux-gnu --with-ssl --disable-shared --enable-libcurl-option --enable-static
</code></pre>
