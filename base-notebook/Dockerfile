# Copyright (c) Xingetouzi Development Team.
# Distributed under the terms of the Modified BSD License.

FROM jupyter/scipy-notebook

USER root

# update apt repo
RUN sed -i 's/archive.ubuntu.com/mirrors.ustc.edu.cn/g' /etc/apt/sources.list && apt-get update && apt-get install tzdata && apt-get clean

# TA-lib dependencies
RUN wget -O ta-lib.tar.gz http://prdownloads.sourceforge.net/ta-lib/ta-lib-0.4.0-src.tar.gz && \
tar -xzf ta-lib.tar.gz &&\
cd ta-lib && ./configure --prefix=/usr && make && make install && cd .. && rm -rf ta-lib*

# update conda settings
RUN conda config --add channels https://mirrors.ustc.edu.cn/anaconda/cloud/conda-forge/ --system && \
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/free/ --system && \
conda config --add channels https://mirrors.ustc.edu.cn/anaconda/pkgs/main/ --system && \
conda config --set show_channel_urls yes --system && \
conda config --set allow_softlinks False --system

USER $NB_UID

# update pip source
RUN mkdir -p /home/$NB_USER/.config/pip && \
echo "[global]" > /home/$NB_USER/.config/pip/pip.conf && \
echo "index-url = https://mirrors.ustc.edu.cn/pypi/web/simple" >> /home/$NB_USER/.config/pip/pip.conf && \
echo "format = columns" >> /home/$NB_USER/.config/pip/pip.conf

# # activate jupyter_contrib
# RUN pip install --no-cache jupyter_contrib_nbextensions && \
# pip install --no-cache jupyter_nbextensions_configurator

# Install Python 3 packages
RUN conda install --quiet --yes \
    'pandas=0.20.0' \
    'ipyparallel=6.0.*' \
    'numba=0.34.*' \
    'nb_conda' && \
    conda clean -tipsy && \
    jupyter nbextension install --sys-prefix --py ipyparallel  &&\
    jupyter nbextension enable --sys-prefix --py ipyparallel  &&\
    jupyter serverextension enable --sys-prefix --py ipyparallel &&\
    pip install --no-cache ${PIP_EXTRA} TA-lib &&\
    npm cache clean && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# RUN jupyter contrib nbextension install --sys-prefix && \
#     jupyter nbextensions_configurator enable --sys-prefix &&\
#     jupyter nbextensions_configurator enable --system