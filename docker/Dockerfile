FROM nvidia/cuda:10.2-base

RUN apt-get update && apt-get install -y vim wget curl git

## set up vim
RUN git clone https://github.com/estorrs/dotfiles.git \
  && cp dotfiles/.vimrc /root/.vimrc \
    && curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

## set up git
RUN git config --global user.email epstorrs@gmail.com \
  && git config --global user.name Erik Storrs

## ## set up conda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-py37_4.9.2-Linux-x86_64.sh -O ~/miniconda.sh
RUN bash ~/miniconda.sh -b -p /miniconda
ENV PATH="/miniconda/bin:$PATH"

COPY . /spatial-analysis
WORKDIR /spatial-analysis

# make sure we use the conda env
RUN conda env create -f /spatial-analysis/environment/spatial.yml
SHELL ["conda", "run", "-n", "spatial", "/bin/bash", "-c"]

RUN conda install -y -c conda-forge xgboost altair
RUN pip install tensorflow==2.1.0

CMD jupyter notebook --no-browser --allow-root --ip 0.0.0.0
