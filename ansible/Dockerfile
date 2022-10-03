ARG BASE_IMAGE
FROM $BASE_IMAGE

ARG APK_REPO
ARG PYPI_REPO

ENV LANG en_US.UTF-8
ENV LC_ALL en_US.UTF-8
ENV PYTHONWARNINGS ignore

COPY ./*.crt /usr/local/share/ca-certificates/
COPY ./marlin /marlin

RUN mkdir -p /etc/ansible && cp /marlin/ansible.cfg /etc/ansible/ansible.cfg

RUN test -z "$APK_REPO" || echo $APK_REPO > /etc/apk/repositories
RUN test -z "$PYPI_REPO" || pip config --global set global.index-url $PYPI_REPO  && \
    pip config --global set global.extra-index-url $PYPI_REPO  && \
    pip config --global set global.trusted-host $(echo $PYPI_REPO | awk -F / '{print $3}')
 

RUN addgroup -g 1001 python && \
    adduser -u 1001 -G python -D python -s /bin/sh && \
    chown -R 1001 /marlin

RUN apk --update --no-cache add gcc libc-dev ca-certificates sshpass wget ca-certificates openssh && \
    update-ca-certificates && \
    su - python -s /bin/sh -c 'python3 -m venv ./ && \
    pip3 install --upgrade pip && \
    pip3 install --upgrade virtualenv && \
    python3 -m pip install --no-cache-dir  requests && \
    python3 -m pip install --no-cache-dir  "ansible==2.10" && \
    python3 -m pip install --no-cache-dir  "ansible-lint" && \
    pip cache purge' && \
    apk del gcc libc-dev && \
    rm -rf /var/cache/apk/* 

ENV PATH="${PATH}:/home/python/.local/bin"

USER 1001

WORKDIR /marlin 

