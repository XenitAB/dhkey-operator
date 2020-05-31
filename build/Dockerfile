FROM quay.io/operator-framework/ansible-operator:v0.18.0

COPY requirements.yml ${HOME}/requirements.yml
RUN ansible-galaxy collection install -r ${HOME}/requirements.yml \
 && chmod -R ug+rwx ${HOME}/.ansible

USER root

RUN  yum -y update \
 && yum -y install openssl \
 && yum clean all \
 && rm -rf /var/cache/yum

COPY requirements.txt ${HOME}/requirements.txt

RUN pip3 install --no-cache-dir --ignore-installed -r ${HOME}/requirements.txt

USER 1001

COPY watches.yaml ${HOME}/watches.yaml

COPY roles/ ${HOME}/roles/
