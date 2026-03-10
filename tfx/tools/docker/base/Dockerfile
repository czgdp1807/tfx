# Copyright 2020 Google LLC
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#      http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# Base image used to facilitate docker building.
# This gets updated nightly.

FROM ubuntu:22.04
LABEL maintainer="tensorflow-extended-dev@googlegroups.com"

ARG DEBIAN_FRONTEND=noninteractive
ARG APT_COMMAND="apt-get -o Acquire::Retries=3 -y"

# 1. Initial update and upgrade
RUN ${APT_COMMAND} update && \
    ${APT_COMMAND} -qq upgrade

# 2. Install software-properties-common for add-apt-repository
RUN ${APT_COMMAND} -qq install --no-install-recommends software-properties-common

# 4. Install build tools and Python 3.10
RUN ${APT_COMMAND} -qq install --no-install-recommends \
    build-essential \
    ca-certificates \
    libsnappy-dev \
    protobuf-compiler \
    libprotobuf-dev \
    wget \
    unzip \
    git

# 5. Install Miniconda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda && \
    rm ~/miniconda.sh

ENV PATH="/opt/conda/bin:$PATH"
RUN conda config --set auto_update_conda false && \
    conda config --set show_channel_urls true && \
    conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main && \
    conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r && \
    conda init bash && \
    conda update -n base -c defaults conda && \
    conda clean --all -f -y

# 7. Clean up apt
RUN ${APT_COMMAND} autoclean && \
    ${APT_COMMAND} autoremove --purge && \
    rm -rf /var/lib/apt/lists/*

