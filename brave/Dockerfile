FROM ubuntu:18.04
LABEL maintainer="admin@tor.com"
LABEL version="0.1"
LABEL description="This help you build tor with single command on any enviroment"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt install -y wget git build-essential
RUN apt install -y build-essential libgnome-keyring-dev python-setuptools npm
RUN apt install -y libyaml-libyaml-perl libtemplate-perl libdatetime-perl \
                   libio-handle-util-perl libio-all-perl \
                   libio-captureoutput-perl libjson-perl libpath-tiny-perl \
                   libstring-shellquote-perl libsort-versions-perl \
                   libdigest-sha-perl libdata-uuid-perl libdata-dump-perl \
                   libfile-copy-recursive-perl libfile-slurp-perl git \
                   uidmap
RUN curl -sL https://deb.nodesource.com/setup_17.x -o nodesource_setup.sh
RUN bash nodesource_setup.sh
RUN apt install nodejs
RUN rm -rf /var/lib/apt/lists/* 
#RUN sed -ie 's/GRUB_CMDLINE_LINUX=.*/GRUB_CMDLINE_LINUX="systemd.legacy_systemd_cgroup_controller=1 "/' /etc/default/grub
#RUN sed -ie 's/GRUB_CMDLINE_LINUX_DEFAULT=.*/GRUB_CMDLINE_LINUX_DEFAULT="systemd.legacy_systemd_cgroup_controller=1 "/' /etc/default/grub
#RUN update-grub
#RUN sysctl -w kernel.unprivileged_userns_clone=1
# -u 121 -g docker -G docker,adm,systemd-journal,sudo test2
#RUN useradd -m -d /home/test2 test2
#RUN passwd -d test2
#RUN usermod --add-subuids 1100000-1199999 --add-subgids 1100000-1199999 test2
#COPY builder.sh /home
WORKDIR /home
RUN usermod --add-subuids 1000000-1099999 --add-subgids 1000000-1099999 root

RUN git config --global http.postBuffer 524288000 \
    && git config --global https.postBuffer 524288000 \
    && git config --global http.sslVerify false \
    && git config --global https.sslVerify false \
    && git config --global core.compression -1
    
RUN git clone https://github.com/brave/brave-browser.git \
    && cd brave-browser \
    && npm install \
    && npm run init -- --target_os=android --target_arch=arm
