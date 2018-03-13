# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:nbextens

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### nbgrader (https://github.com/jupyter/nbgrader) ####

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends git && \
    rm -rf /var/lib/apt/lists/*

RUN cd /tmp && \
    git clone https://github.com/ipython-contrib/Ipython-notebook-extensions.git && \
    cd Ipython-notebook-extensions && \
    python setup.py install

USER $NB_USER

RUN conda install --quiet --yes -c anaconda nose

RUN conda install --quiet --yes -c conda-forge nbgrader && \
    conda clean -tipsy

RUN jupyter nbextension install --sys-prefix --py nbgrader --overwrite && \
    jupyter nbextension enable --sys-prefix --py nbgrader && \
    jupyter serverextension enable --sys-prefix --py nbgrader

RUN jupyter nbextension disable --sys-prefix formgrader/main --section=tree && \
    jupyter serverextension disable --sys-prefix nbgrader.server_extensions.formgrader && \
    jupyter nbextension disable --sys-prefix create_assignment/main && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN chmod 777 /home/jovyan/work

WORKDIR /home/jovyan/work

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER

