#
# Copyright (c) 2020 Red Hat, Inc.
# This program and the accompanying materials are made
# available under the terms of the Eclipse Public License 2.0
# which is available at https://www.eclipse.org/legal/epl-2.0/
#
# SPDX-License-Identifier: EPL-2.0
#

# https://catalog.redhat.com/software/containers/ubi8/nodejs-12/5d3fff015a13461f5fb8635a?tag=1-70
FROM registry.access.redhat.com/ubi8/nodejs-12:1-70

ARG extension_repository
ARG extension_revision
ARG extension_name

USER root
WORKDIR /

RUN dnf -y install git python36 python3-six python3-pip platform-python-pip \
    curl wget bzip2 gdb make cmake gcc gcc-c++
RUN /usr/bin/python3 -m pip install --upgrade pip

RUN mkdir ./${extension_name}-src && cd ./${extension_name}-src && \
    git clone ${extension_repository} ${extension_name} && \
    cd ./${extension_name} && git checkout ${extension_revision} && \
    rm -rf ./.git && tar -czvf /${extension_name}-${extension_revision}-sources.tar.gz ./ && \
    npm install -g vsce gulp-cli && npm install --unsafe-perm=true --allow-root && \
    gulp installPythonLibs && gulp prePublishBundle && \
    vsce package --out /${extension_name}-${extension_revision}.vsix
