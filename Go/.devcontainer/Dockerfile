FROM golang:1.22.2-bullseye
# FROM qmcgaw/godevcontainer:debian

# RUN apt-get install -y mercurial
# RUN apt-get install -y make
# RUN apt-get install -y binutils
# RUN apt-get install -y mercurial
# RUN apt-get install -y make
# RUN apt-get install -y binutils
# RUN apt-get install -y bison
# RUN apt-get install -y gcc
# RUN apt-get install -y build-essential


# CA certificates
RUN apt-get update -y && \
apt-get install -y --no-install-recommends ca-certificates && \
rm -r /var/cache/* /var/lib/apt/lists/*

# Timezone
RUN apt-get update -y && \
apt-get install -y --no-install-recommends tzdata && \
rm -r /var/cache/* /var/lib/apt/lists/*
ENV TZ=

RUN apt-get -y update
RUN apt-get -y install mc
RUN apt-get -y install htop

# Setup Git and SSH
# Workaround for older Debian in order to be able to sign commits
RUN echo "deb https://deb.debian.org/debian bookworm main" >> /etc/apt/sources.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends -t bookworm git git-man && \
    rm -r /var/cache/* /var/lib/apt/lists/*
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends man openssh-client less && \
    rm -r /var/cache/* /var/lib/apt/lists/*
COPY .ssh.sh /root/
RUN chmod +x /root/.ssh.sh

# Setup shell
ENTRYPOINT [ "/bin/zsh" ]
RUN apt-get update -y && \
    apt-get install -y --no-install-recommends zsh nano locales wget && \
    apt-get autoremove -y && \
    apt-get clean -y && \
    rm -r /var/cache/* /var/lib/apt/lists/*
ENV EDITOR=nano \
    LANG=en_US.UTF-8 \
    TERM=xterm
RUN echo "LC_ALL=en_US.UTF-8" >> /etc/environment && \
    echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen && \
    echo "LANG=en_US.UTF-8" > /etc/locale.conf && \
    locale-gen en_US.UTF-8
RUN usermod --shell /bin/zsh root

COPY shell/.zshrc shell/.welcome.sh /root/
RUN git clone --single-branch --depth 1 https://github.com/robbyrussell/oh-my-zsh.git ~/.oh-my-zsh

ARG POWERLEVEL10K_VERSION=v1.16.1
COPY shell/.p10k.zsh /root/
RUN git clone --branch ${POWERLEVEL10K_VERSION} --single-branch --depth 1 https://github.com/romkatv/powerlevel10k.git ~/.oh-my-zsh/custom/themes/powerlevel10k && \
    rm -rf ~/.oh-my-zsh/custom/themes/powerlevel10k/.git
