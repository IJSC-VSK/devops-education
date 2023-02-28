FROM library/alpine:3.8
RUN mkdir -p ~/.ssh/ && chmod 700 ~/.ssh && echo -e "Host *\n\tStrictHostKeyChecking no\n\n" > ~/.ssh/config
 
RUN apk add --allow-untrusted --no-cache \
            bash \
            ca-certificates \
            curl \
            gettext \
            gettext-libs \
            libssh2 \
            libcurl \
            libattr \
            libacl \
            libedit \
            libgomp \
            libintl \
            libunistring \
            libxml2 \
            nghttp2-libs \
            ncurses-terminfo \
            ncurses-terminfo-base \
            ncurses-libs \
            openssh-client \
            openssh-keygen \
            popt \
            readline \
            rsync \
            xz-libs \
 && rm -rf /var/cache/apk/* && rm -f /*.apk

