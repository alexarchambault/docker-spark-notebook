########################################################################
# Dockerfile for Jupyter notebooks with python, pySpark & R support
#
#                    ##        .
#              ## ## ##       ==
#           ## ## ## ##      ===
#       /""""""""""""""""\___/ ===
#  ~~~ {~~ ~~~~ ~~~ ~~~~ ~~ ~ /  ===- ~~~
#       \______ o          __/
#         \    \        __/
#          \____\______/
#
# Component:    spark-notebook
# Author:       pjan vandaele <pjan@pjan.io>
# Scm url:      https://github.com/docxs/docker-cassandra
# License:      MIT
########################################################################

# Pull base image
FROM \
  docxs/spark:1.4

# Maintainer details
MAINTAINER \
  pjan vandaele "pjan@pjan.io"

# Set environment variables
ENV PYTHON_VERSION 3.4.3
ENV OPENBLAS_VERSION 0.2.14
ENV PYTHON_NUMPY_VERSION 1.9.2
ENV PYTHON_SCIPY_VERSION 0.15.1

# Add configuration files
ADD config /src/config

# Install the whole shebang
RUN apt-prepare \
 && echo "Installing the necessary packages ..." \
 && echo "deb http://http.debian.net/debian wheezy-backports main" >> /etc/apt/sources.list \
 && curl -sL https://deb.nodesource.com/setup_0.12 | bash - \
 && apt-get install -q -y --no-install-recommends \
      build-essential \
      gfortran \
      git-core \
      pandoc \
      sqlite3 \
      npm \
      nodejs\
      libatlas3-base \
      libxml2-dev \
      libpq-dev \
      libssl-dev \
      zlib1g-dev \
      libsqlite3-dev \
      libzmq3-dev \
      libcurl4-openssl-dev \
      libfreetype6-dev \
      libhdf5-dev \
      libpng-dev \
 && echo "Installing python ..." \
 && apt-get remove -q -y --purge \
      python \
      python-minimal \
      python2.7 \
      python2.7-dev \
      python2.7-minimal \
 && curl -sL https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz | tar xzf - -C /opt/ \
 && cd /opt/Python-${PYTHON_VERSION} \
 && ./configure \
 && make \
 && make install \
 && ln -s /usr/local/bin/python3 /usr/bin/python \
 && pip3 install --upgrade pip \
 && pip3 install cython \
 && echo "Installing jupyter ..." \
 && mkdir -p /opt/jupyter \
 && cd /opt/jupyter \
 && git clone https://github.com/jupyter/notebook.git \
 && cd notebook \
 && pip3 install -r requirements.txt -e . \
 && python3 -m ipykernel.kernelspec \
 && echo "Installing openBLAS ..." \
 && cd /tmp \
 && git clone -q --branch=master git://github.com/xianyi/OpenBLAS.git \
 && cd OpenBLAS/ \
 && git checkout tags/v${OPENBLAS_VERSION} \
 && make DYNAMIC_ARCH=1 NO_AFFINITY=1 NUM_THREADS=32 \
 && make install \
 && export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/ \
 && echo "Installing numpy ..." \
 && cd /tmp \
 && git clone -q --branch=master git://github.com/numpy/numpy.git \
 && cd numpy \
 && git checkout tags/v${PYTHON_NUMPY_VERSION} \
 && cp /src/config/numpy-site.cfg /tmp/numpy/site.cfg \
 && python3 setup.py install \
 && echo "Installing scipy ..." \
 && cd /tmp \
 && git clone -q --branch=master git://github.com/scipy/scipy.git \
 && cd scipy \
 && git checkout tags/v${PYTHON_SCIPY_VERSION} \
 && cp /src/config/scipy-site.cfg /tmp/scipy/site.cfg \
 && ldconfig && export LD_LIBRARY_PATH=/opt/OpenBLAS/lib/ \
 && python3 setup.py install \
 && echo "Installing additional python packages ..." \
 && pip3 install \
      pandas \
      scikit-learn \
      scikit-image \
      matplotlib \
      seaborn \
      h5py \
      yt \
      sympy \
      patsy \
      statsmodels \
      ggplot \
      vincent \
      dill \
      networkx \
      tabulate \
      pymongo \
      redis \
      psycopg2 \
      cloudant \
 && echo "Installing tex support ..." \
 && apt-get install -q -y --no-install-recommends \
      texlive \
      texlive-latex-extra \
 && echo "Cleaning up ..." \
 && rm -rf /tmp/* \
 && apt-get remove -y --purge \
      libxml2-dev \
      libpq-dev \
      libssl-dev \
      zlib1g-dev \
      libsqlite3-dev \
      libzmq3-dev \
      libcurl4-openssl-dev \
      libfreetype6-dev \
      libhdf5-dev \
      libpng-dev \
 && apt-cleanup \
 && mv /bin/bootstrap /bin/bootstrap-spark

# Add the binary files
ADD bin /bin/

# expose the relevant ports
EXPOSE \
  8888

# Run bootstrap on container launch
ENTRYPOINT \
  ["bootstrap"]

# Default command
CMD \
  ["-d"]
