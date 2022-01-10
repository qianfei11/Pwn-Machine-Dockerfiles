FROM alpine

# update apk
RUN apk update && apk add --no-cache bash gcc g++ musl-dev make cmake git tmux vim lxterminal && \
    echo -e "syntax enable\n\
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
    echo -e "set-option -g default-shell /bin/zsh\n\
bind r source-file ~/.tmux.conf \; display-message 'Config reloaded..'\n\
unbind '\"'\n\
bind - splitw -v -c '#{pane_current_path}'\n\
unbind %\n\
bind \\\\\\\\ splitw -h -c '#{pane_current_path}'\n\
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
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/lib/apk/* /etc/apk/cache/*

# install ohmyzsh
RUN apk update && apk add --no-cache zsh curl wget && \
    sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" && \
    echo -e "\nzstyle ':omz:update' mode disabled" >> ~/.zshrc && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/lib/apk/* /etc/apk/cache/*

# pwntools
RUN apk update && apk add --no-cache python3 py3-pip nasm socat netcat-openbsd python3-dev libffi-dev openssl-dev capstone && \
    python3 -m pip install --no-cache-dir pip wheel setuptools unicorn -U && \
    CRYPTOGRAPHY_DONT_BUILD_RUST=1 python3 -m pip install --no-cache-dir pwntools ipython -U && \
    echo -e "\nalias objdump='objdump -M intel'\nexport PATH='$HOME/.local/bin:$PATH'" >> ~/.zshrc && \
    echo -e "[update]\ninterval=never" > ~/.pwn.conf && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/lib/apk/* /etc/apk/cache/*

# ruby
RUN apk update && apk add --no-cache ruby ruby-dev && \
    gem install one_gadget seccomp-tools && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/lib/apk/* /etc/apk/cache/*

# gdb plugin
RUN apk update && apk add --no-cache gdb && \
    git clone https://github.com/longld/peda.git ~/peda && \
    git clone https://github.com/scwuaptx/Pwngdb.git ~/Pwngdb && cp ~/Pwngdb/.gdbinit ~/ && \
    rm -rf /tmp/* /var/tmp/* /var/cache/apk/* /var/lib/apk/* /etc/apk/cache/*

ENV LANG C.UTF-8

VOLUME ["/pwn"]
WORKDIR /pwn

CMD ["/bin/zsh"]
