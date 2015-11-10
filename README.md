# MCServers
Linux Minecraft server management service script.

The script originated from the year4000 script made by @ewizid.
https://github.com/Year4000/StartScript

However, with the current version of year4000 listed on their github, if your linux server has updated and made changes to start, restart, or status service commands, the script is nearly useless.
"service year4000 stop" would result in the linux server stopping the whole script and all servers. You couldn't tell it to stop just one server

With MCServers, I've fixed it, though with an additional command.
"service mcservers server stop 2/all" results in the server with the ID Number 2 to be stopped, or the choice of all, with visual information and countdown on screen and on the server announcement in sync.

#Commands:
>>> All commands are ran from linux terminal as sudo! No console or ingame use! 

    Server +
        Start ID#/All
            - Start server ID or All servers. Ignores all servers that are already started or Enabled=false in servers.cfg. Requires # or All to run.
            - If the linux server is booting up, it will start all servers that are enabled. Same as though you've don "service mcservers server start all"
            Example: service mcservers server start all
        Stop ID#/All
            - Stop server ID or All servers. Countdown on screen and on the server, then server shutdown. Requires # or All to run.
            - If the linux server is shutting down, it will stop all servers. Same as though you've don "service mcservers server stop all"
            Example: service mcservers server stop all
        Restart ID#/All
            - Restarts server ID or All servers. Stops the server as shown above, followed by a few seconds before Starting. Startup ignores servers with Enabled-false. Requires # or All to run.
            Example: service mcservers server restart all
        Status (ID#)
            - Shows the online/offline/disabled status of all servers. ID# can be stated for info on one server, but not needed.
            Example: service mcservers server status
    Update +
        Spigot/Minecraft/Snapshot/Bungeecord/All +
            - If a URL for the specified update is set to "none," the update for related file(s) will be skipped.
            Example: service mcservers update minecraft
            Clean
                - While updating Spigot, setting the last variable to "clean" will delete all buildtools files, download buildtools, and run it. Starting fresh. Useful if having build issues.
                Example: service mcservers update Spigot/All clean
    Screen ID#
        - View the console/screen of choice server.
        Example: service mcservers screen 0
    Edit FILE-OPTION ID#
        - View/Edit the Servers.cfg, as well as Server.Properties, Bukkit.yml, Spigot.yml, commands.yml, or log of choice server ID#. Can also be used to edit template versions.
        Example: service mcservers edit server.properties 0
    Create FILE-OPTION
        - Create a template file of server.properties, Bukkit.yml, Spigot.yml, and Commands.yml.
        - These templates are pre-coded into mcservers script and can be altered to output default templates of your liking.
        Example: service mcservers create spigot.yml
    Overwrite/Import FILE-OPTION ID#
        - Imports selected template into server choice.
            It's wise to edit the template to your preferences before overwriting, such as bungee, max players, and port settings.
        Example: service mcservers overwrite spigot.yml 0
    Backup ID# +
        - Backup server directory choice, displays the size of choice server.
        Clean Xdays
            Clean out backup directory by filtering out any backups exceeding X number of days.
        Example: service mcservers backup 0


#Features:


    CTRL+C friendly at any moment of server script managing. NOT DURING SCREEN/CONSOLE ACCESS! If a server is starting or shutting down when used, nothing is damaged. The server will continue to start or stop as normal. If a screen/console of a server refuses to shut down, View screen, CTRL+A, hit K, then Y for yes.
    Many help menus for sub-menus to help guide you. Much still under work as I have been adding, switching, and changing more than I would like to keep the help menus updated. The main help menu is up to date 99% of the time.
    Countdown timer displays on server shutdowns.
    Backup entire server directories. This idea came from minebackup, but not nearly as efficient.
        The backup command will tell you the current size of the chosen server directory, and the size of the directory holding all the backups. Best not to let either get too large.
        Has a clean option to clean out any backups exceeding X number of days old. "(sudo) service mcservers backup clean 5" would clean out any backups older than 5 days.
        Planned Feature: Filter to skip certain files or directories upon backup, similar to minebackup.
    Access to server screens/consoles are easily accessible through mcservers "(sudo) service mcservers screen ID#", otherwise "screen -x screenname" Which ever you prefer.
    Terminal is color coded. Easier on the eyes and doesn't look bland. Future plans to allow users to customize the colors. Currently Green, LightGreen, Yellow, and Red are used.
    Many error checks and output messages for success or failures, such as missing files during server startup core file upgrade, or server core.jar missing after update, such as a failed download or buildtools.jar outputting a new spigot_version.jar file.
    Script designed to start and stop all enabled servers upon linux system startup or shutdown. I cannot guarantee the servers will shut down safely and completely before the system shuts down. Please monitor your mc servers shutdown time spand.
    Customize server time of announcements per server. Such as Survival having 20 seconds of shutdown countdown warning, meaning 20 spam message ingame, while another server, such as bungeecord, has 5 seconds.
    Each server group has variables to customize which server runs which server_core.jar copied from root directory.
    Need certain commands ran before starting certain servers? Able to add in your own commands under Server_extras[serverIDvariable]=""
    Instead of commenting out each server group you wish to disable, followed by changing the ID# at the beginning of each. Each server grouping as a Server_enabled variable to state if the server should be allowed to start. Default is True. Set to disabled and the server grouping will be skipped upon startup.
    If the servers.cfg is none-existent, either first time running the service, the servers.cfg is deleted or renamed, a new one will be generated upon next mcservers command run. This is the only time the service script makes any changes to servers.cfg.
    Creation of template files for use of easy server creation and setup with lease number of needed changes. Spigot.yml, bukkit.yml, commands.yml, and server.properties.
    The script's update downloads the jars, while with spigot, it will download and run buildtools.jar. All server core.jar files will be saved to the root directory where servers.cfg is at.


#Installation:


    Copy the mcservers file to /etc/init.d.
    Make the script executable: (sudo) chmod +x mcservers
    Before it can be ran, you will need to make one change to the script.
        Update the SERVER_SETTINGS="" to the user and directory of the Minecraft servers. It is best to have all the Minecraft and Bungee directories in one area.
        The example listed has the Survival and Bungee running under /home/minecraft/MCServers
        This is where all server directories, updates, templates, and servers.cfg are saved.
    Initial run.
        Run "(sudo) service mcservers" and it will generate the servers.cfg file for you in the server_settings set directory. The service will then list the Help menu.
        If successful, no errors. Should all colored text.
    Update servers.cfg.
        Then either "(sudo) service mcservers edit servers.cfg," edit with your preferred editor, or through FTP (and Notepad++). NO TABS! 4 spaces for intentions!
        Make sure that all the server files you want (Buildtools, Minecraft, snapshot, bungeecord) have their respective URLs or set to "none" to be disabled during updates.
        Each server has it's own grouping of variables. Survival server is commented with details of each variable. First line refers to it's ID#, which is shown with "service mcservers server status".
        Make sure the Server_Path refers to the directory name of the server, found in the same directory as servers.cfg.
    Update server core files.
        Run "(sudo) service mcservers update all", or a single choice, to be created/updated. These files will be copied into their respective server directories upon startup.
    Start servers.
        Start one or all servers: "(sudo) service mcservers start all" or replace All with the ID# of which server you'd wish to start.
        If an error appears about unable to stat a file, the update step was either skipped or failed.
    Status Check.
        After a few moments, check the status of the servers for any issues.
        Either this be with "(sudo) service mcservers status", "(sudo) service mcservers view log ID#" or open the screen/console with "(sudo) service mcservers screen ID#."
    (*OPTIONAL*) Want to make the script start ENABLED servers at computer/system startup?
        We need to make the script auto run at startup: "sudo update-rc.d mcservers defaults"
