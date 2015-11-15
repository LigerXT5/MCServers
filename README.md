#README not finalized. Currently a mess. Refer to https://www.spigotmc.org/resources/mcservers-linux-service-script.14209/
# MCServers
Linux Minecraft server management service script.

The script originated from the year4000 script made by @ewizid.
https://github.com/Year4000/StartScript

However, with the current version of year4000 listed on their github, if your linux server has updated and made changes to start, restart, or status service commands, the script is nearly useless.
"service year4000 stop" would result in the linux server stopping the whole script and all servers. You couldn't tell it to stop just one server

With MCServers, I've fixed it, though with an additional command.
"service mcservers server stop 2/all" results in the server with the ID Number 2 to be stopped, or the choice of all, with visual information and countdown on screen and on the server announcement in sync.
