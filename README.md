# Redis Data Integration with Docker compose

Redis Data Integration (RDI) is not released as a docker image but *for demonstration purpose*
this can be of use to run in Docker compose.

* This repo is WIP *

# Usage
```
docker compose build
docker compose up

docker exec -it redis-rdi-postgresql-rdi-1 /bin/bash
./install.sh
# ... interactive installation
# you can setup hostname "redis" and port "6379"
# with default user and no password, no TLS
# for RDI Redis database
# (using Redis Stack for now - wip)
```

# TODO
- silent instal
- combine with RE in docker compose
- add demo scenario
- assess if Redis Stack is best/needed/workable for demo


# Installation log example

From ./install.sh
```
[root@67ec73e21037 1.0.2]# ./install.sh
Welcome to RDI installation. This command will walk you through the installation steps.

RDI installation does not support the ability to create RDI database when:
- Cluster API is using TLS
- RDI database needs to be created using TLS

Would you like the installation to create the RDI Redis database for you? [Y/n]: n
Please enter the RDI Redis database hostname or IP address: redis
Please enter the RDI Redis database port: 6379
Please enter the RDI Redis database username to use or press enter if you are using the default user:
Please enter the RDI Redis database password to use []:
Do you want to use TLS? [y/N]:
Testing RDI database connectivity and space...
Using existing RDI database...
Connected successfully to RDI database.

Installing RDI components. This might take a minute
..
Press select the source database type you want to use:
1: Db2
2: Mysql
3: Oracle
4: Postgresql
5: Sqlserver
Select a database type by index [1]: 4
Scaffolded /opt/rdi/config successfully
Installation completed successfully!

In order to create an RDI pipeline, please edit the /opt/rdi/config/config.yaml configuration file.
You may also add specific transformation jobs in the jobs sub-folder.

RDI needs the source & target database username and password. To set use the following:
    redis-di set-secret SOURCE_DB_USERNAME <username>
    redis-di set-secret SOURCE_DB_PASSWORD <password>
    redis-di set-secret TARGET_DB_USERNAME <username>
    redis-di set-secret TARGET_DB_PASSWORD <password>

In addition you might want to secure the connections to the source and target database
using TLS or mTLS by adding the following:
    redis-di set-secret SOURCE_DB_CACERT <path to source db CA cert>
    redis-di set-secret SOURCE_DB_CERT <path to public key pem file for the client>
    redis-di set-secret SOURCE_DB_KEY <path to private key pem file for the client>
    redis-di set-secret SOURCE_DB_KEY_PASSWORD <private key password>

    redis-di set-secret TARGET_DB_CACERT <path to target db CA cert>
    redis-di set-secret TARGET_DB_CERT <path to public key pem file for the client>
    redis-di set-secret TARGET_DB_KEY <path to private key pem file for the client>
    redis-di set-secret TARGET_DB_KEY_PASSWORD <private key password>


When you are ready, deploy your pipeline using the command:
    redis-di deploy --dir /opt/rdi/config
```

# Known issues

- Dockerfile.ubuntu20 works standalone (docker exec) and with proper RDI install but does not work with Docker compose (bus error) [on an x86 Mac]
- Dockerfile.ubi9 works both standalone (docker exec) and with docker compose [on an x86 Mac, seems to be an issue in M1/M* Mac - to be confirmed]

 


# RANDOM NOTES / WIP below


docker pull ubuntu:20.04
docker run -it --privileged ubuntu:20.04 /bin/bash

apt update
apt install -y curl


mkdir rdi
cd rdi
export RDI_VERSION=0.125.0b6
curl https://qa-onprem.s3.amazonaws.com/redis-di/$RDI_VERSION/rdi-installation-$RDI_VERSION.tar.gz -O
tar -xvf rdi-installation-$RDI_VERSION.tar.gz

cd rdi_install/$RDI_VERSION

```
Would you like the installation to create the RDI Redis database for you? [Y/n]: Y
Please enter the Redis Enterprise Cluster hostname or IP address: host.docker.internal
Please enter the Redis Enterprise Cluster admin API port [9443]:
Please enter the Redis Enterprise Cluster username to use: admin@redis.io
Please enter the Redis Enterprise Cluster password to use []:
Please enter the RDI Redis database password to use:
Please confirm:
Would you like the RDI Redis database to be highly available (recommended for production)? [y/N]: N
Testing RDI database connectivity and space...
Creating new RDI database:
 name: redis-di-1
 port: 12001
 memory: 250MB
 shards: 1
 replication: False
New instance created on port 12001:
 DNS: redis-12001.host.docker.internal
 ADDR: 172.17.0.3
Default Context created successfully
RDI database is successfully installed and connected.
```



CRITICAL - Error while attempting to install RDI:
Command 'INSTALL_K3S_SKIP_DOWNLOAD=true INSTALL_K3S_EXEC='--write-kubeconfig-mode=644 --config=/etc/rancher/k3s/config.yaml' deps/k3s/install.sh' returned non-zero exit status 1.

export DEBIAN_FRONTEND=noninteractive
export TZ="UTC"
apt-get install -y tzdata
ln -fs /usr/share/zoneinfo/Europe/Paris /etc/localtime
dpkg-reconfigure --frontend noninteractive tzdata

apt install systemd
TODO ask TZ data ...

System has not been booted with systemd as init system (PID 1). Can't operate.
Failed to connect to bus: Host is down


Command '['k3s', 'ctr', 'images', 'import', '/rdi/rdi_install/0.125.0b6/images/monitor.tar', 'all']' returned non-zero exit status 1.



/etc/init.d/dbus start


redhat/ubi9-init



Installing RDI components. This might take a minute
CRITICAL - Error while attempting to install RDI:
Command '['k3s', 'ctr', 'images', 'import', '/rdi/rdi_install/0.125.0b6/images/monitor.tar', 'all']' returned non-zero exit status 1.
[root@b4bad947bbc8 0.125.0b6]# k3s ctr images ls
ctr: failed to dial "/run/k3s/containerd/containerd.sock": context deadline exceeded: connection error: desc = "transport: error while dialing: dial unix:///run/k3s/containerd/containerd.sock: timeout"




curl -sfL https://get.k3s.io | sh -
    9  ps aux
   10  kubectl get nodes
   11  k3s check-config
   12  k3s ctr images ls
   13  journalctl -u k3s
   14  ls
   15  pwd
   16  ls
   17  vi /etc/hosts
   18  ping 921ef65b3c2c\
   19  ping 921ef65b3c2c
   20  systemctl stop k3s
   21  systemctl status
   22  systemctl status k3s
   23  systemctl start k3s
   24  systemctl status k3s
   25  journalctl -u k3s



/var/lib/rancher has to be a local filesystem such as ext4 or XFS.

https://github.com/AkihiroSuda/containerized-systemd/blob/master/Dockerfile.ubuntu-20.04

