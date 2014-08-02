# Master
#
# VERSION               0.0.1

FROM    denmat/oracle_base_65
MAINTAINER bkanyid "bradon@kanyid.org"

# Install repos for puppet and dependencies 
RUN rpm -ivh http://yum.puppetlabs.com/el/6/products/i386/puppetlabs-release-6-7.noarch.rpm
RUN rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/i386/epel-release-6-8.noarch.rpm

# Make sure the package repository is up to date
RUN yum -y update 

# Install puppet stuff
RUN yum install -y puppet puppet-server

# Install nice things
RUN yum install -y vim wget curl git tar

# setup sshd
RUN yum install -y openssh-server openssh-clients
RUN echo 'root:root' | chpasswd
RUN sed -i 's/UsePAM yes/UsePAM no/g' /etc/ssh/sshd_config

## Suppress error message 'Could not load host key: ...'
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_rsa_key -C '' -N ''
RUN /usr/bin/ssh-keygen -t rsa -f /etc/ssh/ssh_host_dsa_key -C '' -N ''

# Install some puppet modules.
# Installing apache as an example
RUN puppet module install puppetlabs-apache

ADD ./site.pp /etc/puppet/manifests/
RUN chmod +r /etc/puppet/manifests/site.pp

ADD ./start.sh /root/

EXPOSE 22
CMD ["/bin/bash", "/root/start.sh"]
