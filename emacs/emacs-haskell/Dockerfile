FROM ubuntu:latest

MAINTAINER enthudave

# environment variables
ENV DEBIAN_FRONTEND noninteractive
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 575159689BEFB442
RUN echo 'deb http://download.fpcomplete.com/ubuntu trusty main'|tee /etc/apt/sources.list.d/fpco.list

RUN apt-get update && \
    apt-get install -y \
    emacs24 \
    git \
    wget \
    fontconfig \
    python \
    python-pip \
    cmake \
    build-essential \
    python-dev \
    tmux \
    zsh \ 
    haskell-platform

RUN useradd dev && \
    echo "ALL            ALL = (ALL) NOPASSWD: ALL" >> /etc/sudoers && \
    cp /usr/share/zoneinfo/Europe/Amsterdam /etc/localtime && \
    dpkg-reconfigure locales && \
    locale-gen en_US.UTF-8 && \
    /usr/sbin/update-locale LANG=en_US.UTF-8

WORKDIR /home/dev
ENV HOME /home/dev
ENV LC_ALL en_US.UTF-8

RUN chown -R dev:dev $HOME
USER dev

RUN git clone https://github.com/powerline/fonts /home/dev/fonts && \
    git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d && \
    /home/dev/fonts/install.sh

ADD spacemacs /home/dev/.spacemacs
ADD tmux.conf /home/dev/.tmux.conf
ADD zshrc /home/dev/.zshrc
ADD zshenv /home/dev/zshenv

ENV PATH $HOME/.local/bin:/home/dev/.cabal/bin:$PATH

RUN cabal update && cabal install ghc-mod

CMD ["/bin/bash"]
