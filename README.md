﻿# mangos-classic-docker

This is a docker flow for: https://github.com/cmangos/mangos-classic

This is nowhere near "live-ready".<br>
The code is ugly. I literally copied commands + fixed as i went on -> tried to run it -> repeat -> until it worked.
It was just a nice challenge to see if i got this working.


This is mainly a test (Try, fail. Try again, succeed)
But it works!
On Windows, WSL, Mac, even my Synology NAS.

The process is quite simple, it's done in a few steps and no programming or learning commands needed.

## Prerequisite:

1. Download Docker.
   - Windows machine + Mac: https://www.docker.com/products/docker-desktop/
   - Linux: https://docs.docker.com/desktop/install/linux-install/

2. Download this repository, either git clone or just the .zip file and unzip it.

And you're good to go.


## 1. Compile

1. Copy your WoW 1.12 client files directly into `./compile_wow_classic/wow` so you have `./compile_wow_classic/wow/Wow.exe` and `./compile_wow_classic/wow/Data/` and all of the rest of the files + directories in there.
2. On the command line run:
    - `cd <whatever folder you place this repo in>/compile_wow_classic`
    - `docker compose up`

    This will take a while, especially on a slow machine.<br>
    Once it's done. the container will quit and stop.<br>
    It will place all the files in the `./wow_classic/data` folder


## 2. Start MySQL Docker

1. On the command line run:
    - `cd <whatever folder you place this repo in>/db_wow_classic`
    - `docker compose up`

    This will take a short bit but a lot longer on a slow machine for the first time.<br>
    It will download the MySQL image + the repository files needed to populate the database(s).<br>
    Once everything is imported, it will start MySQL<br>
    The next time you stop/start the pod, it will simply start MySQL.<br>
    <br>
    [Note: To completely start over, delete the volume with: `docker volume rm db_wow_classic_mysql-datavolume`]

## 3. Start Auth + MangoS:

1. On the command line run:
    - `cd <whatever folder you place this repo in>/wow_classic`
    - `docker compose up`

    It will start 2 containers/pods: `wowclassicserver_realmd` & `wowclassicserver_mangosd`.<br>


## 4. Done!

If you want to run commands for Mangos:
- Download Putty.exe
- Set `connection type` to `RAW`
- Enter the hostname to wherever you installed the containers on (localhost if you installed it on the same PC)
- Set Port to 3443
- Connect!

It will prompt you to a username + pasword. By default the administrator is "administrator" and password is "administrator".
You can find all the commands online, example here: https://github.com/dkpminus/mangos-gm-commands



As for editing the database:<br>
Connect with some MySQL client of your choice to connect to the Docker container that's running wowclassicdb. It should have a local IP as well.

