# Copyright 2020 Efabless Corporation
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

# syntax = docker/dockerfile:1.0-experimental
FROM almalinux:8 as build

# Common development tools and libraries (kitchen sink approach)
RUN yum groupinstall -y "Development Tools"

# magic dependencies
RUN yum install -y csh wget tcl-devel tk-devel libX11-devel cairo-devel ncurses-devel

#General utils
RUN yum install -y git tcl tk libjpeg libgomp libXext libSM libXft libffi cairo gettext Xvfb
RUN yum install -y python3 python3-pip python3-tkinter python3-devel

RUN git clone git://opencircuitdesign.com/magic

WORKDIR magic

# build
RUN ./configure --prefix=/build && \
    make -j4 && \
    make install


ENV MAGIC_ROOT=/magic_root
ENV BUILD_PATH=/build/

ENV PATH=$MAGIC_ROOT:$MAGIC_ROOT/scripts:$BUILD_PATH/bin:$BUILD_PATH/bin/Linux-x86_64:$BUILD_PATH/pdn/scripts:$PATH
ENV LD_LIBRARY_PATH=$BUILD_PATH/lib:$BUILD_PATH/lib/Linux-x86_64:$LD_LIBRARY_PATH
ENV MANPATH=$BUILD_PATH/share/man:$MANPATH


RUN mkdir -p /build/version/

RUN date +"Build Timestamp: %Y-%m-%d_%H-%M-%S" > /build/version/magic.version
RUN git rev-parse HEAD >> /build/version/magic.version
RUN tar -czf /build.tar.gz /build

WORKDIR /magic_root
