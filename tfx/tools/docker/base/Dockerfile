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

# Use a newer LTS Ubuntu version
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
    python3.10-dev \
    python3-pip \
    wget \
    unzip \
    git

# 5. Set Python 3.10 as the default
RUN update-alternatives --install /usr/bin/python python /usr/bin/python3.10 1 && \
    update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.10 1

# 6. Clean up apt
RUN ${APT_COMMAND} autoclean && \
    ${APT_COMMAND} autoremove --purge && \
    rm -rf /var/lib/apt/lists/*

# Install/Upgrade pip
RUN python3 -m pip install --upgrade --pre pip

# Install bazel using bazelisk
RUN wget -O /bin/bazel https://github.com/bazelbuild/bazelisk/releases/download/v1.19.0/bazelisk-linux-amd64 && \
  chmod +x /bin/bazel && \
  bazel version

# Clean up pip cache
RUN rm -rf /root/.cache/pip
