FROM --platform=linux/amd64 centos:8

RUN cd /etc/yum.repos.d/
RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
#Update
RUN yum update -y

# Dependencies
RUN yum -y install wget

#Install Python 3
RUN dnf install -y python3
RUN dnf install -y  python2 python2-chardet
RUN dnf install -y policycoreutils-python-utils



# Install yum repository
#RUN cd /etc/yum.repos.d && wget https://research.cs.wisc.edu/htcondor/yum/repo.d/htcondor-stable-rhel8.repo

# Import signing key
#RUN wget http://research.cs.wisc.edu/htcondor/yum/RPM-GPG-KEY-HTCondor && rpm --import RPM-GPG-KEY-HTCondor

RUN  dnf --enablerepo=powertools install boost-python3 -y
# Install HTCondor
#RUN yum -y install condor

RUN yum install -y https://research.cs.wisc.edu/htcondor/repo/9.x/htcondor-release-current.el8.noarch.rpm
RUN yum install -y condor
# Install Nano
RUN yum -y install nano

COPY condor_config.local /etc/condor/
COPY schedd /etc/condor/config.d/
COPY htcondor-schedd-liveness /usr/local/bin/

RUN adduser marco
RUN mkdir /home/marco/wf

# Install Java 1.8.0
RUN yum install -y java-1.8.0-openjdk-devel

#Install jq
RUN yum install -y jq

#Install pegasus
#RUN git clone https://github.com/pegasus-isi/pegasus.git

RUN curl --output /etc/yum.repos.d/pegasus.repo https://download.pegasus.isi.edu/pegasus/rhel/8/pegasus.repo
RUN dnf install -y epel-release
RUN dnf install -y --enablerepo powertools pegasus

# Install golang
RUN wget https://storage.googleapis.com/golang/getgo/installer_linux
RUN chmod +x ./installer_linux
RUN SHELL=$SHELL ./installer_linux                         
RUN source ~/.bash_profile
COPY docker-entrypoint.sh /
EXPOSE 8080
ENTRYPOINT ["/docker-entrypoint.sh"]
WORKDIR /home/marco/wf