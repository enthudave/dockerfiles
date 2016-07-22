FROM phusion:0.9.19

MAINTAINER enthudave

# environment variables
# ENV DEBIAN_FRONTEND noninteractive

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
    python-dev

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

RUN git clone https://github.com/VundleVim/Vundle.vim.git /home/dev/.vim/bundle/Vundle.vim && \ 
    git clone https://github.com/powerline/fonts /home/dev/fonts && \
    /home/dev/fonts/install.sh && \
    vim +PluginInstall +qall && \ 
    rm $HOME/.vimrc

RUN /home/dev/.vim/bundle/YouCompleteMe/install.py

CMD ["/bin/bash"]
