FROM buildpack-deps:xenial-curl  
  
# rpm is required for FPM to build rpm package  
# yasm is required to build p7zip  
# osslsigncode to sign windows on Linux  
# install modern multi-thread xz  
# ldconfig - see 4.6. liblzma.so (or similar) not found when running xz  
ENV XZ_VERSION 5.2.2  
# we don't use our bundled 7za because it is better to build for specific
platform - not generic  
ENV USE_SYSTEM_7ZA true  
  
ENV DEBUG_COLORS true  
ENV FORCE_COLOR true  
  
RUN curl -L https://yarnpkg.com/latest.tar.gz | tar xvz && mv dist yarn && ln
-s /yarn/bin/yarn /usr/local/bin/yarn && apt-get update -y && \  
apt-get install --no-install-recommends -y git snapcraft xorriso bsdtar build-
essential autoconf libssl-dev icnsutils graphicsmagick gcc-multilib
g++-multilib libgnome-keyring-dev lzip rpm yasm && \  
apt-get clean && \  
rm -rf /var/lib/apt/lists/* && \  
curl -L http://tukaani.org/xz/xz-$XZ_VERSION.tar.xz | tar -xJ && cd
xz-$XZ_VERSION && ./configure && make && make install && cd .. && rm -rf
xz-$XZ_VERSION && ldconfig && \  
mkdir -p /tmp/7z && curl -L
http://downloads.sourceforge.net/project/p7zip/p7zip/16.02/p7zip_16.02_src_all.tar.bz2
| tar -xj -C /tmp/7z --strip-components 1 && cd /tmp/7z && cp
makefile.linux_amd64_asm makefile.machine && make && make install && rm -rf
/tmp/7z  
  
# nodejs keys  
RUN set -ex \  
&& for key in \  
9554F04D7259F04124DE6B476D5A82AC7E37093B \  
94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \  
0034A06D9D9B0064CE8ADF6BF1747F4AD2306D93 \  
FD3A5288F042B6850C66B31F09FE44734EB7990E \  
71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \  
DD8F2338BAE7501E3DD5AC78C273792F7D83545D \  
B9AE9905FFD7803F25714661B63B535A4C206CA9 \  
C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \  
; do \  
gpg --keyserver ha.pool.sks-keyservers.net --recv-keys "$key"; \  
done  
  
WORKDIR /project  
  
# fix error /usr/local/bundle/gems/fpm-1.5.0/lib/fpm/package/freebsd.rb:72:in
`encode': "\xE2" from ASCII-8BIT to UTF-8 (Encoding::UndefinedConversionError)  
# http://jaredmarkell.com/docker-and-locales/  
# http://askubuntu.com/a/601498  
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8  
ENV PATH "$HOME/.yarn/bin:$PATH"  
ENV NODE_VERSION 6.9.1  
# https://github.com/npm/npm/issues/4531  
RUN curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-
linux-x64.tar.xz" \  
&& curl -SLO "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \  
&& gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \  
&& grep " node-v$NODE_VERSION-linux-x64.tar.xz\$" SHASUMS256.txt | sha256sum
-c - \  
&& tar -xJf "node-v$NODE_VERSION-linux-x64.tar.xz" -C /usr/local \--strip-
components=1 \  
&& rm "node-v$NODE_VERSION-linux-x64.tar.xz" SHASUMS256.txt.asc SHASUMS256.txt
\  
&& curl -L https://npmjs.org/install.sh | sh && npm cache clean && npm config
set unsafe-perm true && npm completion >> ~/.bashrc  
  
# libgnome-keyring-dev — to build keytar  
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F9CB8DB0 && \  
echo "deb http://ppa.launchpad.net/ubuntu-wine/ppa/ubuntu xenial main " | tee
/etc/apt/sources.list.d/wine.list && \  
dpkg --add-architecture i386 && \  
apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys
3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF && \  
echo "deb http://download.mono-project.com/repo/debian wheezy main" | tee
/etc/apt/sources.list.d/mono-xamarin.list && \  
apt-get update -y && \  
apt-get install -y --no-install-recommends wine1.8 mono-devel ca-certificates-
mono && \  
apt-get clean && rm -rf /var/lib/apt/lists/*  
  
ENV WINEDEBUG -all,err+all  
ENV WINEDLLOVERRIDES winemenubuilder.exe=d  
  
RUN wineboot --init || true  
  

