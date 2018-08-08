# piwigo

Piwigo photo server

```
#### TAG=5.6.10
TAG=latest

# docker rm -f piwigo kibana
# rm -rf /pv/elk
mkdir -p /pv/mariadb
chmod -R 777 /pv/mariadb
chcon -R system_u:object_r:svirt_sandbox_file_t:s0 /pv/mariadb

mkdir -p /pv/piwigo
chmod -R 777 /pv/piwigo
chcon -R system_u:object_r:svirt_sandbox_file_t:s0 /pv/piwigo

# https://hub.docker.com/_/mariadb/
# docker network create -d bridge --subnet 172.25.0.0/16 isolated_nw
#

docker run --network=isolated_nw --ip=172.25.3.3 --restart=always --name=mariadb --detach -e MYSQL_ROOT_PASSWORD=piwigo \
       -e MYSQL_DATABASE=piwigo -e MYSQL_USER=piwigo -e MYSQL_PASSWORD=piwigo -p 3306:3306 \
        -v /pv/mariadb:/var/lib/mysql docker.io/mariadb:$TAG 

docker run --network=isolated_nw --ip=172.25.3.4 --restart=always --name=piwigo --detach -p 9001:80 --link mariadb:mariadb -v /pv/piwigo:/config \
        docker.io/linuxserver/piwigo:$TAG

```
