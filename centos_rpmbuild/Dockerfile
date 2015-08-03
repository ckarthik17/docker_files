#Thanks to y-ken for the base file
FROM centos:centos6

MAINTAINER Karthik Chandraraj "https://github.com/ckarthik17"

RUN yum -y install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum -y groupinstall 'Development Tools'
RUN yum -y install git

RUN yum -y install rpm-build
RUN yum -y install yum-utils
RUN yum -y install rpmdevtools

# Setup sshd to accept login
RUN yum -y install openssh-server
RUN sed -ri 's/UsePAM yes/#UsePAM yes/g' /etc/ssh/sshd_config
RUN sed -ri 's/#UsePAM no/UsePAM no/g' /etc/ssh/sshd_config
RUN /etc/init.d/sshd start
RUN /etc/init.d/sshd stop
EXPOSE 22

# Setup rpm build configuration for root user
RUN rpmdev-setuptree
RUN echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros

# Setup rpm build configuration for general user 'build'
RUN yum -y install sudo
RUN sed -i 's/requiretty/!requiretty/' /etc/sudoers
RUN echo 'build ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers
RUN useradd -d /home/build -p `openssl passwd -1 "build"` build
RUN sudo -ubuild rpmdev-setuptree
RUN sudo -ubuild echo '%_topdir %(echo $HOME)/rpmbuild' > ~/.rpmmacros

# Set environment variables
ENV HOME /root
WORKDIR /root/rpmbuild

# Set default `docker run` command behavior
CMD /usr/sbin/sshd -D
