# Copyright (c) Computational Intelligence PK Team.

FROM amarszalek/wdsi-env:scipytf

LABEL maintainer="Adam Marszalek <amarszalek@pk.edu.pl>"

#### Prolog ####

USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    libarchive13 \
    libgmp10 \
    libossp-uuid16 \
    ca-certificates \
    libdb5.3 \
    libpcre3 \
    libedit2 \
    libgeos-c1v5 \
    libspatialindex4v5 \
    unixodbc \
    odbc-postgresql \
    tdsodbc \
    libsqlite3-0 \
    libserd-0-0 \
    libraptor2-0 && \
    rm -rf /var/lib/apt/lists/*
RUN set -eux; \
    SWIPL_VER=7.6.3; \
    SWIPL_CHECKSUM=9c0be513dc98e6ca420d109f7a455a6593840682cf1b0e21876d1e71e7f35d39; \
    BUILD_DEPS='make gcc g++ wget git autoconf libarchive-dev libgmp-dev libossp-uuid-dev libpcre3-dev libreadline-dev libedit-dev libssl-dev zlib1g-dev libdb-dev unixodbc-dev libsqlite3-dev librocksdb-dev libserd-dev libraptor2-dev libgeos++-dev libspatialindex-dev'; \
    apt-get update; apt-get install -y --no-install-recommends $BUILD_DEPS; rm -rf /var/lib/apt/lists/*; \
    mkdir /tmp/src; \
    cd /tmp/src; \
    wget http://www.swi-prolog.org/download/stable/src/swipl-$SWIPL_VER.tar.gz; \
    echo "$SWIPL_CHECKSUM  swipl-$SWIPL_VER.tar.gz" >> swipl-$SWIPL_VER.tar.gz-CHECKSUM; \
    sha256sum -c swipl-$SWIPL_VER.tar.gz-CHECKSUM; \
    tar -xzf swipl-$SWIPL_VER.tar.gz; \
    cd swipl-$SWIPL_VER; \
    sed -e '/PREFIX=$HOME/c\PREFIX=/usr' \
        -e '/# export DISABLE_PKGS/c\export DISABLE_PKGS="jpl xpce"' \
        -e 's/# *\(EXTRA.*--disable-libdirversion\)/\1/' \
           build.templ > build; \
    chmod u+x build; ./build; \
    mkdir -p /usr/lib/swipl/pack; \
    cd /usr/lib/swipl/pack; \
    rm -rf /tmp/src; \
    # usage: install_addin addin-name git-url git-commit
    install_addin () { \
        git clone "$2" "$1"; \
        git -C "$1" checkout -q "$3"; \
        if [ "$1" = 'space' ]; then (cd "$1"; ln -s configure.ac configure.in); fi; \
        # the prosqlite plugin lib directory must be removed?
        if [ "$1" = 'prosqlite' ]; then rm -rf "$1/lib"; fi; \
        swipl -g "pack_rebuild($1)" -t halt; \
        find "$1" -mindepth 1 -maxdepth 1 ! -name lib ! -name prolog ! -name pack.pl -exec rm -rf {} +; \
        find "$1" -name .git -exec rm -rf {} +; \
    }; \
    install_addin space https://github.com/JanWielemaker/space.git cd6fefa63317a7a6effb61a1c5aee634ebe2ca05; \
    install_addin prosqlite https://github.com/nicos-angelopoulos/prosqlite.git a1d915d07933ece27ea5fd68f07c83d10583e7a0; \
    install_addin rocksdb https://github.com/JanWielemaker/rocksdb.git 29eaee6fcdb6dce690ed187ef68b80ee94739412; \
    install_addin hdt https://github.com/JanWielemaker/hdt.git e0a0eff87fc3318434cb493690c570e1255ed30e; \
    install_addin rserve_client https://github.com/JanWielemaker/rserve_client.git 72838bbfa3976a83d19fb38bdae04378e30f4b0d; \
    apt-get purge -y --auto-remove $BUILD_DEPS

#### iprolog (https://github.com/matsubara0507/iprolog) ####

USER root

RUN apt-get update && apt-get install -y --no-install-recommends gprolog && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

RUN conda install --quiet --yes ipython jupyter && \
    conda clean -tipsy && \
    fix-permissions $CONDA_DIR && \
    fix-permissions $HOME

RUN mkdir /tmp/iprolog
ADD . /tmp/iprolog

RUN cd /tmp/iprolog/kernels && jupyter kernelspec install prolog && \
    fix-permissions $CONDA_DIR && \
    fix-permissions /tmp/iprolog && \
    fix-permissions $HOME

# Switch back to nbuser to avoid accidental container runs as root
USER $NB_USER
