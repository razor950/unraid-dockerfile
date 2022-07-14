# Use archlinux:latest as parent image
FROM archlinux:latest

# Set maintainer
LABEL maintainer='r00tz <razor950@gmail.com>'

# Update mirrors and packages. Install jupyterlab
RUN pacman --noconfirm -Syyu python-pip jupyterlab base-devel cmake npm nodejs
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y
ENV PATH="/root/.cargo/bin:${PATH}"
RUN cargo install evcxr_jupyter --version 0.13.0 && evcxr_jupyter --install

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
