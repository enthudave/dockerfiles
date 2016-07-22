FROM enthudave/develop-base

RUN apt-get update && apt-get install -y \
    vim-nox

RUN cp /home/dev/dotfiles/linux/vim/vimplugins /home/dev/.vimrc

# Install Vim-Plugins
RUN git clone https://github.com/VundleVim/Vundle.vim.git /home/dev/.vim/bundle/Vundle.vim && \
    vim +PluginInstall +qall && \
    cp /home/dev/dotfiles/linux/vim/vimrc /home/dev/.vimrc

RUN /home/dev/.vim/bundle/YouCompleteMe/install.py

USER root
ADD service /etc/service
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
