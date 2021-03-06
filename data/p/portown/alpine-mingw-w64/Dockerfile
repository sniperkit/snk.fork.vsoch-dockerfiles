FROM alpine:3.7  
  
ARG MINGW_ARCH=x86_64  
  
ARG BINUTILS_VERSION=2.29.1  
ENV BINUTILS_DOWNLOAD_URL
http://ftp.gnu.org/gnu/binutils/binutils-$BINUTILS_VERSION.tar.xz  
ENV BINUTILS_DOWNLOAD_SIG
http://ftp.gnu.org/gnu/binutils/binutils-$BINUTILS_VERSION.tar.xz.sig  
ENV BINUTILS_KEY 13FCEF89DD9E3C4F  
  
ARG MINGW_VERSION=5.0.3  
ENV MINGW_DOWNLOAD_URL
https://sourceforge.net/projects/mingw-w64/files/mingw-w64/mingw-w64-release/mingw-w64-v$MINGW_VERSION.tar.bz2/download  
ENV MINGW_DOWNLOAD_SHA1 96278378b829695007ce6a527278cba19cb829f2  
ENV MINGW_ROOT /usr/local/mingw  
  
ARG GCC_VERSION=7.3.0  
ENV GCC_DOWNLOAD_URL
http://ftp.tsukuba.wide.ad.jp/software/gcc/releases/gcc-$GCC_VERSION/gcc-$GCC_VERSION.tar.xz  
ENV GCC_DOWNLOAD_SHA512
ad41a7e4584e40e92cdf860bc0288500fbaf5dfb7e8c3fcabe9eba809c87bcfa85b46c19c19921b0cdf6d05483faede8287bb9ea120c0d1559449a70e602c8d4  
  
ENV PATH $MINGW_ROOT/bin:$PATH  
  
RUN apk add --no-cache --virtual download-dependencies \  
curl \  
gnupg \  
&& mkdir -p /mingw-build \  
&& cd /mingw-build \  
# Download binutils  
&& curl -fsSL "$BINUTILS_DOWNLOAD_URL" -o binutils.tar.xz \  
&& curl -fsSL "$BINUTILS_DOWNLOAD_SIG" -o binutils.tar.xz.sig \  
&& gpg --batch --keyserver pgp.mit.edu `if test "x$HTTP_PROXY" != "x"; then
echo "--keyserver-options http-proxy=$HTTP_PROXY"; fi` --recv-keys
"$BINUTILS_KEY" \  
&& gpg --batch --verify binutils.tar.xz.sig binutils.tar.xz \  
&& gpg --batch --yes \--delete-keys "$BINUTILS_KEY" && rm -Rf /root/.gnupg \  
&& rm -f binutils.tar.xz.sig \  
# Download MinGW-w64  
&& curl -fsSL "$MINGW_DOWNLOAD_URL" -o mingw.tar.bz2 \  
&& echo "$MINGW_DOWNLOAD_SHA1 mingw.tar.bz2" | sha1sum -c - \  
# Download GCC  
&& curl -fsSL "$GCC_DOWNLOAD_URL" -o gcc.tar.xz \  
&& echo "$GCC_DOWNLOAD_SHA512 gcc.tar.xz" | sha512sum -c - \  
&& apk del --purge download-dependencies \  
# Install build dependencies  
&& apk add --no-cache --virtual build-dependencies \  
build-base \  
flex \  
bison \  
gmp-dev \  
mpfr-dev \  
mpc1-dev \  
cloog-dev \  
file \  
&& apk add --no-cache \  
gmp \  
mpfr3 \  
mpc1 \  
cloog \  
# Build binutils  
&& tar -xJf binutils.tar.xz && rm -f binutils.tar.xz \  
&& mkdir build \  
&& ( cd build && CFLAGS="-Os -march=native" CXXFLAGS="-Os -march=native"
../binutils-$BINUTILS_VERSION/configure \  
\--prefix=$MINGW_ROOT \  
\--with-sysroot=$MINGW_ROOT \  
\--target=$MINGW_ARCH-w64-mingw32 \  
\--disable-maintainer-mode \  
\--disable-multilib \  
\--enable-lto \  
\--disable-werror \  
&& make && make install && cd .. ) \  
&& rm -Rf build/ binutils-$BINUTILS_VERSION/ \  
# Install MinGW-w64 headers  
&& tar -xjf mingw.tar.bz2 && rm -f mingw.tar.bz2 \  
&& mkdir build \  
&& ( cd build && ../mingw-w64-v$MINGW_VERSION/mingw-w64-headers/configure
--prefix=$MINGW_ROOT/$MINGW_ARCH-w64-mingw32 --enable-sdk=all --enable-secure-
api --host=$MINGW_ARCH-w64-mingw32 && make install && cd .. ) \  
&& rm -Rf build/ \  
&& ln -s $MINGW_ROOT/$MINGW_ARCH-w64-mingw32 $MINGW_ROOT/mingw \  
# Build GCC stage1  
&& tar -xJf gcc.tar.xz && rm -f gcc.tar.xz \  
&& mkdir gcc-build \  
&& ( cd gcc-build && CFLAGS="-Os -march=native" CXXFLAGS="-Os -march=native"
../gcc-$GCC_VERSION/configure \  
\--target=$MINGW_ARCH-w64-mingw32 \  
\--disable-multilib \  
\--enable-64bit \  
\--prefix=$MINGW_ROOT \  
\--with-sysroot=$MINGW_ROOT \  
\--enable-shared \  
\--with-dwarf \  
\--enable-fully-dynamic-string \  
\--enable-languages=c,c++ \  
\--enable-libssp \  
\--with-host-libstdcxx="-lstdc++ -lsupc++" \  
\--with-gmp=/usr \  
\--with-mpfr=/usr \  
\--with-mpc=/usr \  
\--with-cloog=/usr \  
\--enable-lto \  
\--disable-maintainer-mode \  
\--disable-werror \  
&& make all-gcc && make install-gcc \  
&& cd .. ) \  
# Build MinGW-w64 CRT  
&& mkdir build \  
&& ( cd build && CFLAGS="-Os" CXXFLAGS="-Os"
../mingw-w64-v$MINGW_VERSION/mingw-w64-crt/configure
--prefix=$MINGW_ROOT/$MINGW_ARCH-w64-mingw32 --enable-lib`if [ $MINGW_ARCH =
x86_64 ]; then echo 64; else echo 32; fi` --host=$MINGW_ARCH-w64-mingw32 &&
make && make install && cd .. ) \  
&& rm -Rf build/ mingw-w64-v$MINGW_VERSION/ \  
# Build GCC stage2  
&& ( cd gcc-build && make all-target-libgcc && make install-target-libgcc &&
cd .. ) \  
# Build GCC  
&& ( cd gcc-build && make && make install && cd .. ) \  
&& rm -Rf gcc-build/ gcc-$GCC_VERSION/ \  
&& apk del --purge build-dependencies \  
&& cd / \  
&& rm -Rf /mingw-build/  

