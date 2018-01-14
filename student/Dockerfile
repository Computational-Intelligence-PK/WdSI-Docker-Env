# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:nbextens

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### nbgrader (https://github.com/jupyter/nbgrader) ####

USER $NB_USER

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


VOLUME ["/home/nbuser"]

# Switch back to student to avoid accidental container runs as root
USER $NB_USER

