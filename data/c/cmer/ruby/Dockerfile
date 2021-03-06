FROM buildpack-deps:jessie  
  
# skip installing gem documentation  
RUN mkdir -p /usr/local/etc \  
&& { \  
echo 'install: --no-document'; \  
echo 'update: --no-document'; \  
} >> /usr/local/etc/gemrc  
  
ENV RUBY_MAJOR 2.4  
ENV RUBY_VERSION 2.4.2  
ENV RUBY_DOWNLOAD_SHA256
748a8980d30141bd1a4124e11745bb105b436fb1890826e0d2b9ea31af27f735  
ENV RUBYGEMS_VERSION 2.6.14  
# some of ruby's build scripts are written in ruby  
# we purge system ruby later to make sure our final image uses what we just
built  
RUN set -ex \  
\  
&& buildDeps=' \  
bison \  
dpkg-dev \  
libgdbm-dev \  
ruby \  
' \  
&& devDeps=' \  
xvfb \  
xauth \  
qt5-default \  
libqt5webkit5-dev \  
gstreamer1.0-plugins-base \  
gstreamer1.0-tools \  
gstreamer1.0-x \  
openssh-client \  
nodejs \  
google-chrome-stable \  
' \  
&& wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-
key add - \  
&& echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main"
>> /etc/apt/sources.list.d/google-chrome.list \  
&& apt-get update \  
&& apt-get install -y --no-install-recommends apt-utils \  
&& apt-get install -y --no-install-recommends $buildDeps \  
&& apt-get install -y --no-install-recommends $devDeps \  
&& rm -rf /var/lib/apt/lists/* \  
\  
&& wget -O ruby.tar.xz "https://cache.ruby-
lang.org/pub/ruby/${RUBY_MAJOR%-rc}/ruby-$RUBY_VERSION.tar.xz" \  
&& echo "$RUBY_DOWNLOAD_SHA256 *ruby.tar.xz" | sha256sum -c - \  
\  
&& mkdir -p /usr/src/ruby \  
&& tar -xJf ruby.tar.xz -C /usr/src/ruby --strip-components=1 \  
&& rm ruby.tar.xz \  
\  
&& cd /usr/src/ruby \  
\  
# hack in "ENABLE_PATH_CHECK" disabling to suppress:  
# warning: Insecure world writable dir  
&& { \  
echo '#define ENABLE_PATH_CHECK 0'; \  
echo; \  
cat file.c; \  
} > file.c.new \  
&& mv file.c.new file.c \  
\  
&& autoconf \  
&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \  
&& ./configure \  
\--build="$gnuArch" \  
\--disable-install-doc \  
\--enable-shared \  
&& make -j "$(nproc)" \  
&& make install \  
\  
&& apt-get purge -y --auto-remove $buildDeps \  
&& cd / \  
&& rm -r /usr/src/ruby \  
\  
&& gem update --system "$RUBYGEMS_VERSION"  
ENV BUNDLER_VERSION 1.15.4  
RUN gem install bundler --version "$BUNDLER_VERSION"  
  
# install things globally, for great justice  
# and don't create ".bundle" in all our apps  
ENV GEM_HOME /usr/local/bundle  
ENV BUNDLE_PATH="$GEM_HOME" \  
BUNDLE_BIN="$GEM_HOME/bin" \  
BUNDLE_SILENCE_ROOT_WARNING=1 \  
BUNDLE_APP_CONFIG="$GEM_HOME"  
ENV PATH $BUNDLE_BIN:$PATH  
RUN mkdir -p "$GEM_HOME" "$BUNDLE_BIN" \  
&& chmod 777 "$GEM_HOME" "$BUNDLE_BIN"  
  
CMD [ "irb" ]  

