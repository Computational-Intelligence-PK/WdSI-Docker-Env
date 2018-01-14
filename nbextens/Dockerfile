# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:prolog

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### nbzip ####

USER root

RUN pip install nbzip

RUN jupyter nbextension install --py nbzip && \
    jupyter nbextension enable --py nbzip && \
    jupyter serverextension enable --sys-prefix --py nbzip && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

#### nbgdrive and nbresuse ####

RUN mkdir /tmp/nbext
ADD . /tmp/nbext

RUN cd /tmp/nbext/nbgdrive && pip install .

RUN jupyter serverextension enable --py nbgdrive && \
    jupyter nbextension install --py nbgdrive && \
    jupyter nbextension enable --py nbgdrive && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /tmp/nbext && \
    fix-permissions $HOME

RUN cd /tmp/nbext/nbresuse && pip install .

RUN jupyter serverextension enable --py nbresuse && \
    jupyter nbextension install --py nbresuse && \
    jupyter nbextension enable --py nbresuse && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /tmp/nbext && \
    fix-permissions $HOME


# Switch back to student to avoid accidental container runs as root
USER $NB_USER

