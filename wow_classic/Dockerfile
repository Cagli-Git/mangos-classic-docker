FROM ubuntu:latest AS builder
# RUN dpkg-reconfigure -f noninteractive tzdata
RUN DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y screen mysql-client libmysqlclient21 git gosu inetutils-ping
WORKDIR /cmangos/bin
CMD echo "Use docker-compose to run any command"
