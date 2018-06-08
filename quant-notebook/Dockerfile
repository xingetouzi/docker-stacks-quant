# Copyright (c) Xingetouzi Development Team.
# Distributed under the terms of the Modified BSD License.

FROM daocloud.io/xingetouzi/base-notebook

# Install Python 3 packages
RUN conda install --quiet --yes \
    'python-snappy' \
    'h5py' \
    'lxml' &&\
    conda clean -tipsy && \
    npm cache clean && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /home/$NB_USER

# install all extra packages at last to ensure cache works well
COPY ./requirements.txt /tmp/requirements.txt

ARG PIP_EXTRA

RUN pip install --no-cache ${PIP_EXTRA} -r /tmp/requirements.txt
