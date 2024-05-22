FROM ubuntu

RUN apt-get update && apt-get install wget -y && apt install git -y

# Download and install Miniconda
ENV MINICONDA_VERSION=latest
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-${MINICONDA_VERSION}-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh && \
    ln -s /opt/conda/etc/profile.d/conda.sh /etc/profile.d/conda.sh && \
    echo ". /opt/conda/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

# Set path to conda
ENV PATH /opt/conda/bin:$PATH

# Create conda environment named "mojolang"
RUN conda create -n mojolang python=3.10 -y

RUN conda run -n mojolang pip install jupyter numpy scipy matplotlib ipykernel

RUN curl -s https://get.modular.com | sh -
RUN conda run -n mojolang modular install mojo
RUN conda run -n mojolang modular install nightly/mojo

ENV MOJO_PATH=$(modular config mojo.path)
RUN BASHRC=$( [ -f "$HOME/.bash_profile" ] && echo "$HOME/.bash_profile" || echo "$HOME/.bashrc" ) \
  && echo 'export MODULAR_HOME="'$HOME'/.modular"' >> "$BASHRC" \
  && echo 'export PATH="'$MOJO_PATH'/bin:$PATH"' >> "$BASHRC" \
  && source "$BASHRC"

# # Activate the ngsolve environment
# SHELL ["/bin/bash", "--login", "-c"]
# ENV CONDA_PREFIX /opt/conda/envs/mojolang
# ENV PATH $CONDA_PREFIX/bin:$PATH

## Install any additional packages you need in the mojolang environment
# RUN conda install -n mojolang <package1> <package2> ... -y

# Set the working directory
WORKDIR /workspaces/mojo-setup