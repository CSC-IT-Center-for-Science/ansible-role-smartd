# Latest version of centos
FROM centos:centos7
MAINTAINER James Cuzella <james.cuzella@lyraphase.com>
RUN yum clean all && \
    yum -y install epel-release && \
    yum makecache all && \
    yum -y groups mark convert && \
    yum -y groups mark install "Development Tools" && \
    yum -y groups mark convert "Development Tools" && \
    yum -y groupinstall "Development Tools" && \
    yum -y install libffi-devel openssl-devel dbus-devel dbus-python-devel dbus dbus-glib dbus-glib-devel && \
    yum -y install python-devel MySQL-python sshpass && \
    yum -y install acl sudo && \
    sed -i -e 's/^Defaults.*requiretty/Defaults    !requiretty/' -e 's/^%wheel.*ALL$/%wheel    ALL=(ALL)    NOPASSWD: ALL/' /etc/sudoers && \
    yum -y install PyYAML python-jinja2 python-httplib2 python-keyczar python-setuptools git python-pip && \
    pip install --upgrade pip && \
    pip install requests[security] && \
    pip install pbr==1.1.0 os-service-types==1.2.0 oslo.serialization==1.6.0 oslo.utils==2.0.0 oslo.i18n==1.7.0 \
    debtcollector==0.5.0 python-keystoneclient==1.6.0 oslo.config==1.12.0 stevedore==1.5.0 && \
    pip install pyrax pysphere boto boto3 passlib dnspython && \
    sh -c 'yum -y remove libffi-devel || yum -y --setopt=tsflags=noscripts remove libffi-devel' && \
    yum -y remove $(rpm -qa "*-devel") && \
    yum -y groupremove "Development tools" && \
    yum -y autoremove && \
    yum -y install bzip2 crontabs file findutils gem git gzip hg procps-ng svn sudo tar tree which unzip xz zip && \
    yum clean all && rm -rf /var/cache/yum

RUN mkdir /etc/ansible/
RUN echo -e '[local]\nlocalhost\n' > /etc/ansible/hosts
RUN mkdir /opt/ansible/
RUN git clone http://github.com/ansible/ansible.git /opt/ansible/ansible
WORKDIR /opt/ansible/ansible
RUN git submodule update --init
ENV PATH /opt/ansible/ansible/bin:/bin:/usr/bin:/sbin:/usr/sbin
ENV PYTHONPATH /opt/ansible/ansible/lib
ENV ANSIBLE_LIBRARY /opt/ansible/ansible/library
# Workaround bug in pip / pylockfile: http://pad.lv/1472101
# If HOME is a volume mount, causes infinite loop in pip trying to write lock
ENV XDG_CACHE_HOME /tmp/
RUN source /opt/ansible/ansible/hacking/env-setup


CMD /bin/bash
