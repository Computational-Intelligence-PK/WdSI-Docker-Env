# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:nbextens

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### nbgrader (https://github.com/jupyter/nbgrader) ####

USER $NB_USER

RUN conda install --quiet --yes -c conda-forge nbgrader && \
    conda clean -tipsy

RUN jupyter nbextension install --sys-prefix --py nbgrader --overwrite && \
    jupyter nbextension enable --sys-prefix --py nbgrader && \
    jupyter serverextension enable --sys-prefix --py nbgrader && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HO


VOLUME ["/home/nbuser"]

# Switch back to student to avoid accidental container runs as root
USER $NB_USER
