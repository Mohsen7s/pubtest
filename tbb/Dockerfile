FROM ubuntu:20.04
LABEL maintainer="some@fag.com"
LABEL version="0.1"
LABEL description="This help you build tor with single command on any enviroment, keep in mind 22 is in alpha"
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update
RUN apt install -y wget git build-essential vim curl
RUN apt install -y libyaml-libyaml-perl libtemplate-perl libdatetime-perl \
                   libio-handle-util-perl libio-all-perl \
                   libio-captureoutput-perl libjson-perl libpath-tiny-perl \
                   libstring-shellquote-perl libsort-versions-perl \
                   libdigest-sha-perl libdata-uuid-perl libdata-dump-perl \
                   libfile-copy-recursive-perl libfile-slurp-perl git \
                   uidmap

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
    
RUN git clone https://gitlab.torproject.org/tpo/applications/tor-browser-build.git \
    && cd tor-browser-build \
    && make submodule-update \
    && cd rbm \
    && rm -fr container \
    && wget https://gitlab.torproject.org/tpo/applications/rbm/-/raw/cad40965ce1204936a0df71c405bf0393b534ef9/container \
    && chmod 777 container \
    && cd .. \
    && make fetch \
    && make cargo_vendor-application-services \
    && make cargo_vendor-cbindgen-android \
    && make cargo_vendor-cbindgen \
    && make cargo_vendor-lucetc \
    && make cargo_vendor-uniffi-rs \
    && make get_gradle_dependencies_list-application-services
    

    
#./rbm/rbm build release --target release --target torbrowser-windows-x86_64
#make release-windows-x86_64'

#ENTRYPOINT ["/usr/bin/bash", "/home/builder.sh"]
