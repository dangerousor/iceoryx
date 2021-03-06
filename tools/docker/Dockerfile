# Copyright (c) 2019-2020 by Robert Bosch GmbH. All rights reserved.
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

# Copyright (c) 2019 by Cornel Izbasa. All rights reserved.
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

FROM ubuntu:bionic
ARG REVISION
ARG B_ICEORYX_BUILD
ENV ICEORYX_BUILD=$B_ICEORYX_BUILD

RUN apt update && apt install -y \
        cmake \
        openjdk-11-jdk \
        maven \
        libacl1-dev \
        libncurses5-dev \
        pkg-config \
        screen \
        fish \
        git

# Configureation
## Maven Proxy - required for usage from behind a corporate proxy. Corporate proxy should be bound to docker network at 172.17.0.1.
## If you are not behind a corporate proxy this can be ignored and shouldn't interfere with regular usage.
RUN mkdir -p /root/.m2
RUN echo '\n\
<settings xmlns="http://maven.apache.org/SETTINGS/1.0.0" \n\
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" \n\
  xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.0.0 \n\
                  http://maven.apache.org/xsd/settings-1.0.0.xsd"> \n\
  <proxies> \n\
    <proxy> \n\
      <active>true</active> \n\
      <protocol>http</protocol> \n\
      <host>172.17.0.1</host> \n\
      <port>3128</port> \n\
      <nonProxyHosts>127.0.0.1</nonProxyHosts> \n\
    </proxy> \n\
  </proxies> \n\
</settings> \n'\
>> /root/.m2/settings.xml

ADD . /iceoryx
WORKDIR /iceoryx

RUN ./tools/iceoryx_build_test.sh \
    && cp ./tools/docker/.screenrc /root
