#FROM ubuntu:20.04
FROM et-cluster

RUN apt update -y && apt install munge -y && apt-get install mariadb-server -y

ARG DEBIAN_FRONTEND=noninteractive
RUN apt install slurmd slurm-client slurmctld -y
RUN apt install sudo -y && apt install python3.9 python3-pip -y 
#&& useradd -m admin -s /usr/bin/bash -d /home/admin && echo "admin:admin" | chpasswd && adduser admin sudo && echo "admin     ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers 

# RUN apt update -y && apt install libopenmpi-dev -y && pip3 install mpi4py

COPY slurm.conf /etc/slurm-llnl/
COPY cgroup.conf /etc/slurm-llnl/
COPY docker-entrypoint.sh /etc/slurm-llnl/
COPY relink.sh /

EXPOSE 6817 6818 6819 3306 

# LDAP Setup
RUN apt-get -qq update && \
    apt-get -qq install freeipa-client oddjob-mkhomedir
RUN ipa-client-install
        --mkhomedir
        --server=slurmldap.slurm-cluster.test
        --domain slurm-cluster.test
        --realm SLURM-CLUSTER.TEST
        --unattended
        --no-ntp
        --principal=admin
        --password=78E#s5gcy8HN

WORKDIR /home/admin

ENTRYPOINT ["/etc/slurm-llnl/docker-entrypoint.sh"]
