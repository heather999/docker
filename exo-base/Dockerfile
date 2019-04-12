FROM centos:7 as base
MAINTAINER Heather Kelly <heather@slac.stanford.edu>

FROM base as setup

RUN yum update -y && \
    yum install -y bash \
    bison \
    blas \
    bzip2-devel \
    bzip2 \
    centos-release-scl \
    cmake \
    curl \
    devtoolset-6 \
    flex \
    fontconfig \
    freetype-devel \
    gcc-c++ \
    gcc-gfortran \
    gettext \
    git \
    glibc \
    glibc-devel \
    glib2.0-devel \
    java-1.8.0-openjdk \
    libuuid-devel \
    libselinux \
    libXext \
    libXrender \
    libXt-devel \
    make \
    mesa-libGLU \
    mesa-libGLU-devel \
    ncurses-devel \
    openssl-devel \
    patch \
    perl \
    perl-ExtUtils-MakeMaker \
    readline-devel \
    tar \
    which \
    zlib \
    zlib-devel 

RUN yum clean -y all

RUN rm -rf /var/cache/yum

FROM setup AS installpy

RUN groupadd -g 2000 -r exo && useradd -u 2000 --no-log-init -m -r -g exo exo
ARG EXO_DIR=/opt/exo

USER exo

RUN mkdir -p $EXO_DIR && \
    curl -LO https://repo.continuum.io/miniconda/Miniconda2-4.5.12-Linux-x86_64.sh && \
    source scl_source enable devtoolset-6 && \
    bash ./Miniconda2-4.5.12-Linux-x86_64.sh -b -p /opt/ext/miniconda2 && \
    rm -Rf ./Miniconda2-4.5.12-Linux-x86_64.sh 
    
    
FROM installpy AS installroot

USER exo

RUN cd /opt/exo && \
    mkdir -p /opt/ext/root-src && \
    cd /opt/ext/root-src && \
    curl -LO https://root.cern/download/root_v5.34.38.source.tar.gz && \
    tar xzf root_v5.34.38.source.tar.gz && \
    source scl_source enable devtoolset-6 && \
    cd .. && \
    mkdir -p /opt/exo/root-build && \
    cd /opt/exo/root-build && \
    cmake -Dalien:BOOL=off -Dbonjour:BOOL=OFF -Dbuiltin_freetype=ON -Dbuiltin_pcre:BOOL=ON -Dbuiltin_xrootd:BOOL=ON 
    -Dcxx11:BOOL=OFF -Dcastor:BOOL=OFF -Ddcache:BOOL=OFF -Dfail-on-missing:BOOL=OFF -Dgdml:BOOL=ON -Dgfal:BOOL=OFF 
    -Dglite:BOOL=OFF -Dgsl_shared:BOOL=ON -Dgviz:BOOL=OFF -Dhdfs:BOOL=OFF -Dminuit2:BOOL=ON -Dmonalisa:BOOL=OFF 
    -Dopengl:BOOL=OFF -Dpgsql:BOOL=OFF -Dpythia6:BOOL=OFF -Dpythia8:BOOL=OFF -Drootfit:BOOL=ON -Drfio:BOOL=ON -Dsapdb:BOOL=OFF 
    -Dsrp:BOOL=OFF -Dssl:BOOL=OFF -Dtable:BOOL=ON -Dtesting:BOOL=ON -Dunuran:BOOL=ON -Dx11:BOOL=ON 
    -DPYTHON_EXECUTABLE=/opt/exo/miniconda2/bin/python -DPYTHON_INCLUDE_DIR=/opt/exo/miniconda2/python/2.7.15/include/python2.7 
    -DPYTHON_LIBRARY=/opt/exo/miniconda2/python/2.7.15/lib/libpython2.7.so -DCMAKE_INSTALL_PREFIX=/opt/exo/ROOT/v5.34.38 
    /opt/ext/root-src

