# Stardew Valley EXPANDED Multiplayer Docker Compose

This project aims to autostart a Stardew Valley Expanded Multiplayer Server as
easy as possible.

## Changing Farm Types

To enable different farm types that are supported by Stardew Valley Expanded,
edit your `docker-compose.yml` and enable Grandpa's Farm or Immersive Farm 2.
**Do not enable both of them at once**.

## Side Note

 - Updating to most recent version requires a rebuild: `docker-compose build
   --no-cache` 
 - Although I'm trying to put out updates, I don't have the time for testing,
   so I recommend forking and fixing things on your own.
 - Ansible and Terraform will not be supported anymore
 - Thanks to the brilliant configuration script from Novex and the genius
   baseimage from jlesage this now looks a bit better

## Setup

### Docker-Compose
 
```
git clone https://github.com/humanoidsandvichdispenser/sve-multiplayer-docker
cd sve-multiplayer-docker
docker-compose up
```

## Game Setup

Intially you have to create or load a game once via VNC or Web interface. After
that the Autoload Mod jumps into the previously loaded savegame everytime you
restart or rebuild the container. The AutoLoad Mod config file is by default
mounted as a volume, since it keeps the state of the ongoing SaveGame, but you
can also copy your existing SaveGame to the `Saves` volume and define the
SaveGame's name in the environment variables.

### Adding Other Mods

Place all of your mods in the `docker/mods` directory, **rebuild the
container**, then edit your `docker-compose.yml`.

Your compose file should set environment variables for each mod you want
enabled.

The name of the environment variable is the mod name in the `docker/mods`
directory, allcaps with no spaces. For example, to enable `docker/mods/Tractor
Mod`, you must add the environment variable `ENABLE_TRACTORMOD=true`.

### VNC

Use a vnc client like `TightVNC` on Windows or plain `vncviewer` on any Linux
distribution to connect to the server. You can modify the VNC Port and IP
address and Password in the `docker-compose.yml` file like this:

Localhost:
```
   # Server is only reachable on localhost on port 2342...
   ports:
     - 127.0.0.1:2342:5900
   # ... with the password "insecure"
   environment:
     - VNCPASS=insecure
```

### Web Interface 

On port 5800 inside the container is a web interface. This is a bit easier and
more accessible than just the VNC interface. Although you will be asked for the
vnc password, I wouldn't recommend exposing the port to the outside world.

![img](https://store.eris.cc/uploads/859865e1ab5b23fb223923d9a7e4806b.PNG)

## How it works

The game will be pulled from my (printfuck's) servers (I'll assume you already
own the game - since you're looking for a multiplayer - so please don't rip it
from there) and the modloader (SMAPI) will be pulled from Github when building
the container. You can control the mods's settings with environment variables
in the `docker-compose.yml` file.

## Used Mods

* [AutoLoadGame](https://www.nexusmods.com/stardewvalley/mods/2509)
* [Always On](https://community.playstarbound.com/threads/updating-mods-for-stardew-valley-1-4.156000/page-20#post-3353880)
* [Unlimited Players](https://www.nexusmods.com/stardewvalley/mods/2213)
* some more ...

## Troubleshooting

### Error Messages in Console

Usually you should be able to ignore any message there. If the game doesn't
start or any errors appear, you should look for messages like "cannot open
display", which would most likely indicate permission errors.

### VNC

Access the game via VNC to initially load or start a pregenerated savegame. You
can control the Server from there or edit the config.json files in the configs
folder.

### Performance

I'd recomend a VPS/Machine with at least four logical CPUs and 4GB Ram,
otherwise there will be horrible lags. The minimum configuration I'd consider
playable with two to four players would be two logical CPUs and 1GB of Ram.
