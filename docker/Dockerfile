FROM ubuntu
LABEL maintainer "help@biothings.io"

RUN apt-get -y update
RUN apt-get install -y apt-utils python-yaml python-jinja2 git python3 apt-transport-https curl bash vim tmux sudo npm nodejs-legacy
# some extra packages for debugging purpose, can be commented out for actual image
RUN if [ -z "$PROD" ]; then apt-get install -y htop ne wget net-tools; fi
RUN git clone http://github.com/ansible/ansible.git /tmp/ansible
WORKDIR /tmp/ansible
ENV PATH /tmp/ansible/bin:/sbin:/usr/sbin:/usr/bin:/bin:/usr/local/bin
ENV ANSIBLE_LIBRARY /tmp/ansible/library
ENV PYTHONPATH /tmp/ansible/lib:$PYTHON_PATH

ADD ansible_playbook /tmp/ansible_playbook
ADD inventory /etc/ansible/hosts

WORKDIR /tmp/ansible_playbook
RUN ansible-playbook biothings_studio.yml -c local

# Clean up ansible_playbook
WORKDIR /tmp
RUN if [ -n "$PROD" ]; then rm -rf /tmp/ansible_playbook; fi
RUN if [ -n "$PROD" ]; then rm -rf /tmp/ansible; fi


EXPOSE 8080 9200 7022 7080 27017 22
VOLUME ["/data"]
ENTRYPOINT ["/docker-entrypoint.sh"]