FROM debian:stable

ENV http_proxy="http://192.168.50.4:19700"
ENV https_proxy="http://192.168.50.4:19700"

RUN apt-get update && apt-get install -y ca-certificates

RUN sed -i 's/^Components: main$/& contrib non-free non-free-firmware/' /etc/apt/sources.list.d/debian.sources && \
    sed -i 's/URIs: http:\/\/deb\.debian\.org\/debian/URIs: https:\/\/mirrors.aliyun.com\/debian/' /etc/apt/sources.list.d/debian.sources

ENV http_proxy=""
ENV https_proxy=""
RUN apt-get update

# Install essential tools
RUN apt-get update && apt-get install -y \
    unzip \
    ripgrep \
    fd-find \
    curl \
    wget \
    git \
    build-essential \
    llvm \
    clang \
    libelf-dev \
    pkg-config \
    zsh \
    && apt-get clean

ENV http_proxy="http://192.168.50.4:19700"
ENV https_proxy="http://192.168.50.4:19700"

RUN yes | sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
RUN chsh -s /bin/zsh

# Install neovim
RUN curl -Lv https://github.com/neovim/neovim/releases/download/v0.10.2/nvim-linux64.tar.gz > nvim-linux64.tar.gz && \
    tar zxf nvim-linux64.tar.gz && \
    cp -R ./nvim-linux64/* /usr && \
    rm -rf nvim-linux64*

# Install Rust toolchain
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

ENV PATH="/root/.cargo/bin:${PATH}"

WORKDIR /root

RUN echo 'alias proxy="http_proxy=http://192.168.50.4:19700 https_proxy=http://192.168.50.4:19700"' >> ~/.zshrc
RUN echo 'alias unproxy="http_proxy= https_proxy="' >> ~/.zshrc

RUN mkdir -p ~/dev/github && mkdir -p ~/.config

RUN git clone --depth=1 https://github.com/neevek/dotfiles ~/dev/github/dotfiles && ln -s ~/dev/github/dotfiles/zsh/neevek.zsh-theme ~/.oh-my-zsh/themes/neevek.zsh-theme
RUN git clone --depth=1 https://github.com/neevek/kickstart.nvim ~/.config/nvim
RUN yes | git clone --depth=1 https://github.com/junegunn/fzf.git ~/.fzf && ~/.fzf/install
RUN nvim --headless "+Lazy! sync" +qa
RUN sed -i 's/ZSH_THEME="robbyrussell"/ZSH_THEME="neevek"/' ~/.zshrc

ENV http_proxy=""
ENV https_proxy=""

CMD ["zsh"]

# Build the image
# docker build -t neevek/ebpf-dev .
# Run a container from the image
# docker run -t -d -v /home/neevek/dev/ebpf-dev/data:/root/data --name ebpf-dev1 neevek/ebpf-dev
# Login to the container
# docker exec -it ebpf-dev1 zsh
