#Thanks to y-ken for the base file
FROM centos:centos6

MAINTAINER Karthik Chandraraj "https://github.com/ckarthik17"

RUN yum -y install http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum -y groupinstall 'Development Tools'
RUN yum -y install git

RUN yum -y install rpm-build
RUN yum -y install yum-utils
RUN yum -y install rpmdevtools
RUN yum -y install vim

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

# Apache http server
RUN yum -y install httpd; yum clean all
RUN echo "Apache HTTPD" >> /var/www/html/index.html
EXPOSE 80

RUN wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" -O jdk-linux-x64.rpm "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.rpm"
RUN rpm -Uvh jdk-linux-x64.rpm
RUN rm jdk-linux-x64.rpm
ENV JAVA_HOME /usr/java/default

RUN yum -y install tar
RUN wget http://mirror.nus.edu.sg/apache/maven/maven-3/3.3.3/binaries/apache-maven-3.3.3-bin.tar.gz
RUN tar -xzf apache-maven-3.3.3-bin.tar.gz -C /usr/local

RUN wget http://nodejs.org/dist/v0.12.7/node-v0.12.7-linux-x64.tar.gz
RUN tar --strip-components 1 -xzvf node-v* -C /usr/local
RUN npm install -g shelljs

RUN echo "export NODE_PATH=/usr/local/lib/node_modules" >> /home/build/.bashrc
RUN echo "export M2_HOME=/usr/local/apache-maven-3.3.3" >> /home/build/.bashrc
RUN echo "export PATH=\${M2_HOME}/bin:\${PATH}" >> /home/build/.bashrc

RUN echo "export NODE_PATH=/usr/local/lib/node_modules" >> /root/.bashrc
RUN echo "export M2_HOME=/usr/local/apache-maven-3.3.3" >> /root/.bashrc
RUN echo "export PATH=\${M2_HOME}/bin:\${PATH}" >> /root/.bashrc

# Set environment variables
ENV HOME /root

# Set default `docker run` command behavior
CMD /usr/sbin/sshd -D
