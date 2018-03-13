# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:prolog

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### nbgdrive ####

RUN mkdir $HOME/.nbext
ADD . $HOME/.nbext

#USER root
#
#RUN mv $HOME/.nbext/nbgdrive/gdrive /usr/bin && \
#    fix-permissions /usr/bin
#
#USER $NB_USER
#
#RUN cd $HOME/.nbext/nbgdrive && pip install . && \
#    fix-permissions $CONDA_DIR && \
#    fix-permissions $HOME
#
#RUN jupyter serverextension enable --sys-prefix --py nbgdrive && \
#    jupyter nbextension install --sys-prefix --py nbgdrive && \
#    jupyter nbextension enable --sys-prefix --py nbgdrive && \
#    fix-permissions $CONDA_DIR && \
#    fix-permissions $HOME

#### nbzip (only chrome) ####

USER root

RUN apt-get update && apt-get install -yq --no-install-recommends zip unzip && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/

USER $NB_USER

RUN pip install nbzip && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN jupyter serverextension enable --sys-prefix --py nbzip && \
    jupyter nbextension install --sys-prefix --py nbzip && \
    jupyter nbextension enable --sys-prefix --py nbzip && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

#### nbresuse ####

RUN cd $HOME/.nbext/nbresuse && pip install . && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN jupyter serverextension enable --sys-prefix --py nbresuse && \
    jupyter nbextension install --sys-prefix --py nbresuse && \
    jupyter nbextension enable --sys-prefix --py nbresuse && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN chmod 777 /home/jovyan/work

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER

