FROM ubuntu:latest

MAINTAINER enthudave

# environment variables
ENV DEBIAN_FRONTEND noninteractive
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 575159689BEFB442
RUN echo 'deb http://download.fpcomplete.com/ubuntu trusty main'|tee /etc/apt/sources.list.d/fpco.list

RUN apt-get update && \
    apt-get install -y \
    vim-nox-py2 \
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

ADD vimplugins /home/dev/.vimrc
ADD ycm_extra_conf.py /home/dev/.ycm_extra_conf.py

RUN git clone https://github.com/VundleVim/Vundle.vim.git /home/dev/.vim/bundle/Vundle.vim && \
    git clone https://github.com/powerline/fonts /home/dev/fonts && \
    /home/dev/fonts/install.sh && \
    vim +PluginInstall +qall && \
    rm $HOME/.vimrc

# compiled vimplugins
RUN cd /home/dev/.vim/bundle/vimproc.vim && make
RUN /home/dev/.vim/bundle/YouCompleteMe/install.py

ADD vimrc /home/dev/.vimrc
ADD tmux.conf /home/dev/.tmux.conf
ADD zshrc /home/dev/.zshrc
ADD zshenv /home/dev/zshenv

ENV PATH $HOME/.local/bin:/home/dev/.cabal/bin:$PATH

RUN cabal update && cabal install ghc-mod

CMD ["/bin/bash"]
