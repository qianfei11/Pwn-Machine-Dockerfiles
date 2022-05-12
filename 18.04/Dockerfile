FROM ubuntu:18.04
ARG DEBIAN_FRONTEND=noninteractive

# update apt source
RUN apt-get update && \
    apt-get install -y vim tmux git make cmake lxterminal && \
    echo "syntax enable\n\
filetype plugin indent on\n\
set omnifunc=syntaxcomplete#Complete\n\
set number\n\
set lazyredraw\n\
set showmatch\n\
set ruler\n\
set cursorline\n\
set showcmd\n\
set tabstop=4\n\
set shiftwidth=4\n\
set expandtab\n\
set mouse=a\n\
set clipboard=unnamedplus" > ~/.vimrc && \
    echo "set-option -g default-shell /bin/zsh\n\
bind r source-file ~/.tmux.conf \; display-message 'Config reloaded..'\n\
unbind '\"'\n\
bind - splitw -v -c '#{pane_current_path}'\n\
unbind %\n\
bind \\ splitw -h -c '#{pane_current_path}'\n\
set -g mouse on\n\
#set -g mouse-resize-pane on\n\
#set -g mouse-select-pane on\n\
#set -g mouse-select-window on\n\
#set -g mode-mouse on\n\
set -g default-terminal 'xterm-256color'\n\
bind -r k select-pane -U\n\
bind -r j select-pane -D\n\
bind -r h select-pane -L\n\
bind -r l select-pane -R \n\
setw -g mode-keys vi\n\
set -g status-interval 1\n\
set -g status-justify left\n\
setw -g monitor-activity on\n\
set -g status-bg black\n\
set -g status-fg white\n\
set -g status-left-length 300\n\
set -g status-right-length 500\n\
set -wg window-status-separator ''\n\
set -g status-left '#[fg=cyan, bold] [#(whoami)@#(hostname -s)] #[fg=green, bold]Session: #S #[fg=yellow, bold]#I #[fg=red, bold]#P '\n\
set -g status-right '#[fg=cyan, bold] %Y-%m-%d #[fg=cyan, bold] %H:%M:%S'\n\
setw -g window-status-current-format '#[fg=cyan, bold]*[#I] #W* '\n\
setw -g window-status-format '#[fg=#ffffff, bold] [#I] #W '\n\
setw -g automatic-rename off\n\
setw -g allow-rename off" > ~/.tmux.conf && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install environment
RUN dpkg --add-architecture i386 && \
    apt-get update && \
    apt-get install -y \
    nasm python \
    build-essential \
    python-dev python-pip python-setuptools \
    libc6-dbg \
    libc6-dbg:i386 \
    gcc-multilib g++-multilib \
    glibc-source \
    socat \
    netcat && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# ohmyzsh
RUN apt-get update && \
    apt-get install -y zsh curl wget && \
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    echo "\nzstyle ':omz:update' mode disabled" >> ~/.zshrc && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# pwntools (https://github.com/Gallopsled/pwntools/issues/1984#issuecomment-974645879)
RUN wget https://bootstrap.pypa.io/pip/2.7/get-pip.py && python get-pip.py && rm -f get-pip.py && \
    python -m pip install --no-cache-dir pwntools==4.8.0b0 ipython -U && \
    echo "\nalias objdump='objdump -M intel'\nexport PATH='$HOME/.local/bin:$PATH'" >> ~/.zshrc && \
    echo "[update]\ninterval=never" > ~/.pwn.conf && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# ruby
RUN apt-get update && \
    apt-get install -y ruby ruby-dev && \
    gem install one_gadget seccomp-tools && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# gdb
RUN apt-get update && \
    apt-get install -y ltrace strace texinfo python3 python3-dev libncurses-dev libmpfr-dev && \
    wget https://ftp.gnu.org/gnu/gdb/gdb-9.2.tar.gz -O /tmp/gdb-9.2.tar.gz && \
    tar -zxf /tmp/gdb-9.2.tar.gz -C /tmp/ && \
    cd /tmp/gdb-9.2/ && \
    mkdir /tmp/gdb-9.2/build && cd build/ && ../configure --with-python=python3 && make && make install && \
    rm -rf gdb-9.2.tar.gz gdb-9.2/ && \
    DEBIAN_FRONTEND=noninteractive apt-get remove -y texinfo python3-dev libncurses-dev libmpfr-dev && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# gdb plugin
RUN git clone https://github.com/pwndbg/pwndbg.git ~/pwndbg && cd ~/pwndbg/ && ./setup.sh && \
    git clone https://github.com/scwuaptx/Pwngdb.git ~/Pwngdb && cp ~/Pwngdb/.gdbinit ~/ && \
    sed -i 's/peda\/peda\.py/pwndbg\/gdbinit\.py/g' ~/.gdbinit && \
    apt-get clean && apt-get autoclean && apt-get -y autoremove && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

ENV LANG C.UTF-8

VOLUME ["/pwn"]
WORKDIR /pwn

CMD ["/bin/zsh"]
