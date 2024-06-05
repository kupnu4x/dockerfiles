FROM ubuntu:22.04

RUN DEBIAN_FRONTEND=noninteractive apt-get update && \
    apt-get install -y gnupg2 python3-pip sshpass git openssh-client && \
    rm -rf /var/lib/apt/lists/* && \
    apt-get clean

ARG ANSIBLE_VERSION=7.5.0

RUN python3 -m pip install --upgrade pip cffi && \
    pip3 install ansible==${ANSIBLE_VERSION} && \
    pip3 install mitogen jmespath && \
    pip install --upgrade pywinrm && \
    rm -rf /root/.cache/pip

RUN mkdir -p /etc/ansible && \
    echo 'localhost' > /etc/ansible/hosts

RUN mkdir /home/ansible /nonexistent && \
    chown 65534:65534 /home/ansible /nonexistent

USER 65534:65534
WORKDIR /home/ansible
