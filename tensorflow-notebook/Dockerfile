# Copyright (c) Jupyter Development Team.
# Distributed under the terms of the Modified BSD License.
ARG OWNER=jupyter
ARG BASE_CONTAINER=$OWNER/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="Jupyter Project <jupyter@googlegroups.com>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Install ROCm
USER root
RUN apt update && \
    apt install -y --no-install-recommends wget libnuma-dev gnupg gnupg2 && \
    wget -qO - https://repo.radeon.com/rocm/rocm.gpg.key | apt-key add - && \
    printf "deb [arch=amd64] http://repo.radeon.com/rocm/apt/debian/ jammy main" | tee /etc/apt/sources.list.d/rocm.list  && \
    apt update && \
    apt install -y --no-install-recommends rocm-libs miopen-hip rccl

USER ${NB_UID}

# Install Tensorflow
RUN pip install --no-cache tensorflow-rocm && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"
