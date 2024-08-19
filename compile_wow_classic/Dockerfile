FROM ubuntu:latest AS builder
RUN DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install -y apt-utils build-essential gcc g++ automake git-core autoconf make patch libmysql++-dev libtool libssl-dev grep binutils zlib1g libc6 libbz2-dev cmake subversion libboost-all-dev git gosu mysql-client libmysqlclient21
WORKDIR /cmangos
RUN mkdir build run
RUN git clone https://github.com/cmangos/mangos-classic.git mangos
RUN git clone https://github.com/cmangos/classic-db.git
WORKDIR /cmangos/build
RUN cmake -DDEBUG=0 -DBUILD_EXTRACTORS=ON -DCMAKE_INSTALL_PREFIX=../run ../mangos
RUN make -j`nproc`
RUN make install
WORKDIR /cmangos
COPY wow tmp
RUN mv run/bin/tools/* tmp && rmdir run/bin/tools
WORKDIR /cmangos/tmp
RUN printf "8\ny\ny" | bash ExtractResources.sh a
RUN mv maps dbc Cameras vmaps mmaps ../run/bin
WORKDIR /cmangos

RUN mv /cmangos/run/etc/mangosd.conf.dist /cmangos/run/etc/mangosd.conf
RUN mv /cmangos/run/etc/realmd.conf.dist /cmangos/run/etc/realmd.conf

RUN sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/mangosd.conf && \
    sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/realmd.conf && \
    sed -i 's/Console.Enable = 1/Console.Enable = 0/g' /cmangos/run/etc/mangosd.conf && \
    sed -i 's/Ra.Enable = 0/Ra.Enable = 1/g' /cmangos/run/etc/mangosd.conf

RUN echo "Finished compiling"
WORKDIR /classicmangos/bin

# Done! Placing it all in /classicmangos
CMD rm -rf /classicmangos/* && mv /cmangos/run/* /classicmangos
