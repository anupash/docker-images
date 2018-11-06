# Copyright 2017 Ismail KABOUBI
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

FROM alpine:latest

# Add kubectl to enable sentinels to change labels during promotion
ADD https://storage.googleapis.com/kubernetes-release/release/v1.11.2/bin/linux/amd64/kubectl /usr/local/bin/kubectl

# Copy config files and redis launcher script
COPY redis-master.conf /etc/redis/master.conf
COPY redis-slave.conf /etc/redis/slave.conf
COPY redis-sentinel.conf /etc/redis/sentinel.conf
COPY label-updater.sh /usr/local/bin/label-updater.sh
COPY redis-launcher.sh /usr/local/bin/redis-launcher.sh
COPY promote.sh /usr/local/bin/promote.sh

RUN set -ex \
    \
    && apk add --no-cache 'su-exec>=0.2' \
    \
    && apk update \
    \
    && apk upgrade \
    \
    && apk --update --no-cache --virtual add \
            bash \
            sed \
            curl \
            redis=4.0.11-r0 \
            ca-certificates \
            tzdata \
    \
    && cp /usr/share/zoneinfo/Europe/Berlin /etc/localtime \
    \
    && apk del tzdata \
    \
    && chmod +x /usr/local/bin/kubectl \
    \
    && rm -rf /var/cache/apk/* \
    \
    && touch /var/log/redis/runner.txt \
    \
    && chmod +w /var/log/redis/runner.txt \
    \
    && chmod +x /usr/local/bin/redis-launcher.sh

ENTRYPOINT [ "bash", "-c", "/usr/local/bin/redis-launcher.sh | tee /var/log/redis/runner.txt" ]
