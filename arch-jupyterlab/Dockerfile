# Use archlinux:latest as parent image
FROM archlinux:latest

# Set maintainer
LABEL maintainer='r00tz <razor950@gmail.com>'

# Update mirrors and packages. Install jupyterlab
RUN pacman --noconfirm -Syyu python-pip jupyterlab base-devel cmake npm nodejs lapack file pango cairo zeromq git   

# Sets up Rust and installs the rust juypter kernel
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN cargo install evcxr_jupyter --version 0.13.0 && evcxr_jupyter --install

# Sets up Haskell and installs the haskell juypter kernel
RUN curl -sSL https://get.haskellstack.org/ | sh && cd /opt
RUN git clone https://github.com/gibiansky/IHaskell --depth=1
RUN cd IHaskell && pip3 install -r requirements.txt && stack install --fast 
ENV PATH="/root/.local/bin:${PATH}"
RUN ihaskell install --stack

COPY stack.yaml /root/.stack/global-project/stack.yaml
RUN stack install --fast 

ENV JUPYTERLAB_DIR /appdata/lab

# Build the lab
RUN jupyter lab build --minimize=False

# Expose port and path
EXPOSE 8888
VOLUME /appdata

# Add packages and functionalities
# ENV pkgs=PACKAGES
# CMD pacman --noconfirm -S 

# Run JupyterLab
CMD cp -R -n /usr/share/jupyter/* /appdata && jupyter lab --ip=* --port=8888 --no-browser --notebook-dir=/opt/app/data --allow-root
