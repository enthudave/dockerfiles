FROM phusion/baseimage:0.9.19
ENV DEBIAN_FRONTEND noninteractive
ENV HOME /root

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

RUN apt-get update && apt-get install -y \
    language-pack-en \
    build-essential \
    cmake \
    python \
    python-pip \
    python-dev \
    wget \
    git \
    xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic \
    vim-nox \
#zsh \
    tmux


# Add a non-root user called "dev"
RUN (useradd -m -d /home/dev -s /bin/bash dev && \
     echo "dev ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers)

USER dev
WORKDIR /home/dev
ENV HOME /home/dev
RUN mkdir /home/dev/.vim

# Install oh-my-zsh
#RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

# Install dotfiles
RUN git clone https://github.com/enthudave/dotfiles /home/dev/dotfiles && \
    cp /home/dev/dotfiles/linux/vim/vimplugins /home/dev/.vimrc && \
#    cp /home/dev/dotfiles/linux/zsh/zshrc /home/dev/.zshrc && \
#    cp /home/dev/dotfiles/linux/zsh/zshenv /home/dev/.zshenv && \
    cp /home/dev/dotfiles/linux/tmux/tmux.conf /home/dev/.tmux.conf

# Install Vim-Plugins
RUN git clone https://github.com/VundleVim/Vundle.vim.git /home/dev/.vim/bundle/Vundle.vim && \
    vim +PluginInstall +qall && \
    cp /home/dev/dotfiles/linux/vim/vimrc /home/dev/.vimrc

RUN /home/dev/.vim/bundle/YouCompleteMe/install.py

# Force tmux to use 256 colors to play nicely with vim
RUN echo 'alias tmux="tmux -2"' >> /home/dev/.profile

USER root
ADD service /etc/service
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
