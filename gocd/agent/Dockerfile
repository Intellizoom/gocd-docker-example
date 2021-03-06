# Copyright 2017 ThoughtWorks, Inc.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

###############################################################################################
# This file is autogenerated by the repository at https://github.com/gocd/docker-gocd-agent.
# Please file any issues or PRs at https://github.com/gocd/docker-gocd-agent
###############################################################################################

FROM ubuntu:16.04
MAINTAINER GoCD <go-cd-dev@googlegroups.com>

ARG GOCD_VERSION="17.2.0"
ARG DOWNLOAD_URL="https://download.gocd.io/binaries/17.2.0-4587/generic/go-agent-17.2.0-4587.zip"
ARG GID=1000
ARG UID=1000
ARG NODE_VERSION=7.6

ADD ${DOWNLOAD_URL} /tmp/go-agent.zip
ADD https://github.com/krallin/tini/releases/download/v0.14.0/tini-static-amd64 /usr/local/sbin/tini
ADD https://github.com/tianon/gosu/releases/download/1.10/gosu-amd64 /usr/local/sbin/gosu

# allow mounting ssh keys, dotfiles, and the go server config and data
VOLUME /home/go /godata

# force encoding
ENV LANG=en_US.utf8

RUN chmod 755 /usr/local/sbin/tini /usr/local/sbin/gosu && \
# add our user and group first to make sure their IDs get assigned consistently,
# regardless of whatever dependencies get added
  groupadd -g ${GID} go && \
  useradd -u ${UID} -g go go && \
  echo deb 'http://ppa.launchpad.net/openjdk-r/ppa/ubuntu xenial main' > /etc/apt/sources.list.d/openjdk-ppa.list && \ 
  apt-key adv --keyserver keyserver.ubuntu.com --recv-keys DA1A4A13543B466853BAF164EB9B1D8886F44E2A && \ 
  apt-get update && \ 
  apt-get install -y openjdk-8-jre-headless git openssh-client bash unzip curl && \ 
  apt-get autoclean && \
# unzip the zip file into /go-agent, after stripping the first path prefix
  unzip /tmp/go-agent.zip -d / && \
  mv go-agent-${GOCD_VERSION} /go-agent && \
  rm /tmp/go-agent.zip

RUN curl -sL https://deb.nodesource.com/setup_7.x | bash
RUN apt-get install -y nodejs
RUN chown -R go /home/go && chown -R go /godata

ADD docker-entrypoint.sh /

ENTRYPOINT ["/docker-entrypoint.sh"]

#
# # Exposing volumes in a simple manner, and setup links behind-the-scenes
# # to match GoCD's directory structure
#   mkdir -p ${GO_WORKING_DIR} && \
# #   mkdir -p ${VOLUME_DIR}/pipelines && \
#   ln -sf ${VOLUME_DIR}/pipelines ${GO_WORKING_DIR}/pipelines && \
# #   mkdir -p ${VOLUME_DIR}/config && \
#   ln -sf ${VOLUME_DIR}/config ${GO_WORKING_DIR}/config && \
# #   useradd -m go && \
#   chown -R go:go ${VOLUME_DIR} ${GO_WORKING_DIR}
#
# ADD start-agent.sh /go-agent/start-agent.sh
#
#
# USER go
# ENTRYPOINT ["/usr/local/sbin/tini", "--"]
# WORKDIR ${GO_WORKING_DIR}
# ENV AGENT_WORK_DIR=${GO_WORKING_DIR}
# CMD ["/go-agent/start-agent.sh"]
