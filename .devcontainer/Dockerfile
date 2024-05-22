FROM ubuntu

SHELL ["/bin/bash", "--login", "-c"]

# Specify your conda environment for Mojo programming language 
ARG conda_env_name=mojolang
ARG git_repo_name=mojo-setup

RUN apt-get update && apt-get install wget -y && apt-get install curl -y && apt install git -y

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

# Create conda environment named ${conda_env_name}
RUN conda create -n ${conda_env_name} python=3.10 -y

# Install mojo
RUN curl -s https://get.modular.com | sh -
RUN conda run -n ${conda_env_name} modular install mojo

# Configure mojo after installation
RUN MOJO_PATH=$(modular config mojo.path) \
  && BASHRC=$( [ -f "$HOME/.bash_profile" ] && echo "$HOME/.bash_profile" || echo "$HOME/.bashrc" ) \
  && echo 'export MODULAR_HOME="'$HOME'/.modular"' >> "$BASHRC" \
  && echo 'export PATH="'$MOJO_PATH'/bin:$PATH"' >> "$BASHRC" \
  && source "$BASHRC"


# Activate the ${conda_env_name} environment
ENV CONDA_PREFIX /opt/conda/envs/${conda_env_name}
ENV PATH $CONDA_PREFIX/bin:$PATH
RUN echo "conda activate ${conda_env_name}" >> ~/.bashrc

## Install any additional packages you need in the ${conda_env_name} environment
# RUN conda install -n ${conda_env_name} <package1> <package2> ... -y

# Set the working directory
WORKDIR /workspaces/${git_repo_name}
