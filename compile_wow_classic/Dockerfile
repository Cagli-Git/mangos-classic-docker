FROM ubuntu:latest AS builder
# RUN dpkg-reconfigure -f noninteractive tzdata
RUN DEBIAN_FRONTEND=noninteractive
#RUN apt-get update && apt-get install -y mysql-client libmysqlclient21 git gosu iputils-ping
RUN apt-get update && apt-get install -y apt-utils build-essential gcc g++ automake git-core autoconf make patch libmysql++-dev libtool libssl-dev grep binutils zlib1g libc6 libbz2-dev cmake subversion libboost-all-dev git gosu mysql-client libmysqlclient21
# RUN ping -c 5 wowclassicdb
# RUN mysql -u mangos -h wowclassicdb -pmangos -e "SELECT * FROM mysql.users;"
# TODO: create loop to check if the user exists
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
# RUN sed -i 's/localhost/%/g' /cmangos/mangos/sql/create/db_create_mysql.sql && \
#     mysql -u mangos -h wowclassicdb -pmangos < /cmangos/mangos/sql/create/db_create_mysql.sql && \
#     mysql -u mangos -h wowclassicdb -pmangos classicmangos < /cmangos/mangos/sql/base/mangos.sql && \
#     mysql -u mangos -h wowclassicdb -pmangos classiccharacters < /cmangos/mangos/sql/base/characters.sql && \
#     mysql -u mangos -h wowclassicdb -pmangos classicrealmd < /cmangos/mangos/sql/base/realmd.sql && \
#     printf "MANGOS_DBHOST: wowclassicdb" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MANGOS_DBNAME: classicmangos" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MANGOS_DBUSER: mangos" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MANGOS_DBPASS: mangos" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL_HOST: wowclassicdb" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL_PORT: 3306" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL_USERNAME: mangos" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL_PASSWORD: mangos" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL_PATH: mysql" >> /cmangos/classic-db/InstallFullDB.config && \
#     printf "MYSQL: mysql" > /cmangos/classic-db/InstallFullDB.config && \
#     printf "FORCE_WAIT=NO" > /cmangos/classic-db/InstallFullDB.config && \
#     printf "CORE_PATH=/cmangos/mangos" > /cmangos/classic-db/InstallFullDB.config && \
#     cd classic-db && \
#     bash InstallFullDB.sh

RUN mv /cmangos/run/etc/mangosd.conf.dist /cmangos/run/etc/mangosd.conf
RUN mv /cmangos/run/etc/realmd.conf.dist /cmangos/run/etc/realmd.conf

RUN sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/mangosd.conf && \
    sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/realmd.conf && \
    sed -i 's/Console.Enable = 0/Console.Enable = 1/g' /cmangos/run/etc/mangosd.conf && \
    sed -i 's/Ra.Enable = 0/Ra.Enable = 1/g' /cmangos/run/etc/mangosd.conf

RUN echo "Finished compiling"
WORKDIR /classicmangos/bin

# Done! Placing it all in /classicmangos

# TODO: This whole compiler thing should be in the CMD.
#CMD rm -rf /classicmangos/* && mv /cmangos/run/* /classicmangos
# /cmangos/etc/mangosd.conf
# LoginDatabaseInfo     = "127.0.0.1;3306;mangos;mangos;classicrealmd"
# WorldDatabaseInfo     = "127.0.0.1;3306;mangos;mangos;classicmangos"
# CharacterDatabaseInfo = "127.0.0.1;3306;mangos;mangos;classiccharacters"
# LogsDatabaseInfo      = "127.0.0.1;3306;mangos;mangos;classiclogs"

# /cmangos/etc/realmd.conf
# LoginDatabaseInfo = "127.0.0.1;3306;mangos;mangos;classicrealmd"

# FROM ubuntu:latest
# COPY --from=builder /cmangos/run /cmangos
# # # COPY --from=builder /var/lib/mysql /var/lib/mysql_bak
# RUN apt-get update && apt-get install -y screen mysql-client libmysqlclient21 git gosu
# # # RUN service mysql start
# WORKDIR /classicmangos/bin
# # #CMD screen -dm bash -c "./mangosd" && ./realmd
# # # RUN screen -dm bash -c "./mangosd" && ./realmd
# # #CMD ./run-mangosd
# # #CMD ./run-realmd
# CMD mv /cmangos/* /classicmangos/
# #CMD echo "Finished compiling"




# cmake -DDEBUG=0 -DBUILD_EXTRACTORS=ON -DCMAKE_INSTALL_PREFIX=../run ../mangos
# make -j`nproc`
# make install
# /cmangos
# wow tmp
# mv run/bin/tools/* tmp && rmdir run/bin/tools
# /cmangos/tmp
# printf "8\ny\ny" | bash ExtractResources.sh a
# mv maps dbc Cameras vmaps mmaps ../run/bin
# /cmangos
# mv /cmangos/run/etc/mangosd.conf.dist /cmangos/run/etc/mangosd.conf
# mv /cmangos/run/etc/realmd.conf.dist /cmangos/run/etc/realmd.conf
# sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/mangosd.conf && sed -i 's/127.0.0.1;3306;/wowclassicdb;3306;/g' /cmangos/run/etc/realmd.conf
# echo "Finished compiling"
# /classicmangos/bin

# Done! Placing it all in /classicmangos

# TODO: This whole compiler thing should be in the CMD.
CMD rm -rf /classicmangos/* && mv /cmangos/run/* /classicmangos