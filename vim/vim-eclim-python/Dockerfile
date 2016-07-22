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
    xvfb \
    ant \
    maven \
    xfonts-100dpi xfonts-75dpi xfonts-scalable xfonts-cyrillic \
    vim-nox \
#zsh \
    tmux \
    openjdk-8-jdk


# Add a non-root user called "dev"
RUN (useradd -m -d /home/dev -s /bin/bash dev && \
     echo "dev ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers)

USER dev
WORKDIR /home/dev
ENV HOME /home/dev
RUN mkdir /home/dev/.vim


# Install Eclipse
RUN wget -P /home/dev/ http://ftp.mirrorservice.org/sites/download.eclipse.org/eclipseMirror/technology/epp/downloads/release/mars/R/eclipse-java-mars-R-linux-gtk-x86_64.tar.gz
RUN tar -xzf eclipse-java-mars-R-linux-gtk-x86_64.tar.gz -C /home/dev
RUN rm /home/dev/eclipse-java-mars-R-linux-gtk-x86_64.tar.gz && \
    mkdir /home/dev/workspace && \
    DISPLAY=:1 \
    ./eclipse/eclipse -nosplash -consolelog -debug \
    -application org.eclipse.equinox.p2.director \
    -repository http://download.eclipse.org/releases/mars/ \
    -installIU org.eclipse.dltk.core.feature.group

RUN DISPLAY=:1 ./eclipse/eclipse -nosplash -consolelog -debug \
    -application org.eclipse.equinox.p2.director \
    -repository http://download.eclipse.org/releases/mars/ \
    -installIU org.eclipse.php.feature.group

RUN DISPLAY=:1 \
    ./eclipse/eclipse -nosplash -consolelog -debug \
    -application org.eclipse.equinox.p2.director \
    -repository http://pydev.org/updates/ \
    -installIU org.python.pydev.feature.feature.group


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

# Install eclim
RUN wget -P /home/dev/ http://sourceforge.net/projects/eclim/files/eclim/2.5.0/eclim_2.5.0.jar
RUN java -Dvim.files=$HOME/.vim -Declipse.home=/home/dev/eclipse -jar eclim_2.5.0.jar install

# Force tmux to use 256 colors to play nicely with vim
RUN echo 'alias tmux="tmux -2"' >> /home/dev/.profile && \
    echo 'alias ecd="DISPLAY=:1 /home/dev/eclipse/eclimd"' >> /home/dev/.profile

USER root
ADD service /etc/service
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
