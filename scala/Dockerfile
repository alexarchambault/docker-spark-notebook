########################################################################
# Dockerfile for Jupyter notebooks with python 3, pySpark & R support
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
# Component:    Jupyter with python 3, pySpark and R
# Author:       pjan vandaele <pjan@pjan.io>
# Scm url:      https://github.com/docxs/docker-spark-notebook
# License:      MIT
########################################################################

# Pull base image
FROM \
  docxs/spark-notebook:latest

# Maintainer details
MAINTAINER \
  pjan vandaele "pjan@pjan.io"


# Install Scala kernels
ENV SCALA211_VERSION 2.11.6
ENV SCALA210_VERSION 2.10.5
ENV JUPYTER_SCALA_VERSION 0.2.0-SNAPSHOT

RUN curl -s https://oss.sonatype.org/content/repositories/snapshots/com/github/alexarchambault/jupyter/jupyter-scala-cli_${SCALA211_VERSION}/${JUPYTER_SCALA_VERSION}/jupyter-scala_${SCALA211_VERSION}-${JUPYTER_SCALA_VERSION}.tar.xz | tar -Jx -C /opt/
RUN curl -s https://oss.sonatype.org/content/repositories/snapshots/com/github/alexarchambault/jupyter/jupyter-scala-cli_${SCALA210_VERSION}/${JUPYTER_SCALA_VERSION}/jupyter-scala_${SCALA210_VERSION}-${JUPYTER_SCALA_VERSION}.tar.xz | tar -Jx -C /opt/

RUN /opt/jupyter-scala_${SCALA211_VERSION}-${JUPYTER_SCALA_VERSION}/bin/jupyter-scala
RUN /opt/jupyter-scala_${SCALA210_VERSION}-${JUPYTER_SCALA_VERSION}/bin/jupyter-scala


# Run bootstrap on container launch
ENTRYPOINT \
  ["bootstrap"]

# Default command
CMD \
  ["-d"]
