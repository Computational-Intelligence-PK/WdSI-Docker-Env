# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:minimal

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### Scipy Notebook (https://github.com/jupyter/docker-stacks/tree/master/scipy-notebook) ####

USER root

# libav-tools for matplotlib anim
RUN apt-get update && \
    apt-get install -y --no-install-recommends libav-tools python3-pil python3-pyside python3-xlib python-xlib && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

USER $NB_USER

# Install Python 3 packages
# Remove pyqt and qt pulled in for matplotlib since we're only ever going to
# use notebook-friendly backends in these images
RUN conda install --quiet --yes \
    'nomkl' \
    'ipywidgets=7.0*' \
    'pandas=0.19*' \
    'numexpr=2.6*' \
    'matplotlib=2.0*' \
    'scipy=0.19*' \
    'seaborn=0.7*' \
    'scikit-learn=0.18*' \
    'scikit-image=0.12*' \
    'sympy=1.0*' \
    'cython=0.25*' \
    'patsy=0.4*' \
    'statsmodels=0.8*' \
    'cloudpickle=0.2*' \
    'dill=0.2*' \
    'numba=0.31*' \
    'bokeh=0.12*' \
    'sqlalchemy=1.1*' \
    'hdf5=1.8.17' \
    'h5py=2.6*' \
    'vincent=0.4.*' \
    'beautifulsoup4=4.5.*' \
    'protobuf=3.*' \
    'xlrd'  && \
    conda remove --quiet --yes --force qt pyqt && \
    conda clean -tipsy && \
    # Activate ipywidgets extension in the environment that runs the notebook server
    jupyter nbextension enable --py widgetsnbextension --sys-prefix && \
    # Also activate ipywidgets extension for JupyterLab
    jupyter labextension install @jupyter-widgets/jupyterlab-manager@^0.31.0 && \
    npm cache clean && \
    rm -rf $CONDA_DIR/share/jupyter/lab/staging && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

# Install facets which does not have a pip or conda package at the moment
RUN cd /tmp && \
    git clone https://github.com/PAIR-code/facets.git && \
    cd facets && \
    jupyter nbextension install facets-dist/ --sys-prefix && \
    rm -rf facets && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN cd /tmp && \
    git clone https://github.com/python-xlib/python-xlib.git && \
    cd python-xlib && \
    python setup.py install && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

# Import matplotlib the first time to build the font cache.
ENV XDG_CACHE_HOME /home/$NB_USER/.cache/
RUN MPLBACKEND=Agg python -c "import matplotlib.pyplot" && \
    fix-permissions /home/$NB_USER

RUN pip install deap && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

#### Tensorflow Notebook (https://github.com/jupyter/docker-stacks/blob/master/tensorflow-notebook) ####

# Install Tensorflow and Keras
RUN conda install --quiet --yes \
    'tensorflow=1.4*' \
    'keras=2.0*' && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN conda install --quiet --yes -c conda-forge opencv

USER root

RUN pip install pyscreenshot curtsies keyboard && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN pip install --upgrade mss && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN chmod 777 /home/jovyan/work

# Switch back to jovyan to avoid accidental container runs as root
USER $NB_USER
