# SEC MineCraft Server Operational Document

## Changelog
| Date   |      Description| 
|----------|:-------------:|
| 11/7/20 |  selected a machine and got Ubuntu running |
| 11/18/20 |    created repository for documentation   | 
| 12/18/20 |    Added instructions on setting up bukkit server   | 

## Mission
The Software Engineering Club is proposing a Minecraft server for the club participants to help encourage involvement/networking of students independent of the classroom as well as providing a learning experience, and striving to promote retention in the program.

## Operations

- The system solution is acquired from the stakeholders who are required to realize the server. These are the lab coordinator of the lab where the server is located, the chair of the Software Engineering Program, and FGCU IT Dept. Engagement with these stakeholders is the responsibility of the members of SEC. The members will also define the software and hardware requirements for the server, and verify these have been met.
- Existing requirements and future requirements will be validated by the officers of SEC. The initial solution will be delivered and introduced to members of SEC by the club officers at their discretion.
- The support infrastructure involves having engineering support and maintenance support from the appropriate stakeholders of interest. Training support may be an interactive responsibility given to club members.

## JVM setup
The club currently has a server running the Linux OS with the command line interface. It is recommended that the next course of action is to install OpenJDK 11, which comes with the JVM.
## Minecraft server setup
All information on the computer relating to the Minecraft server shall be stored in a directory called 'minecraft'. If this directory does not exist, one will be created (made manually). 
Inside this directory, the binary libraries for the Minecraft server need to be installed. Use the following command:
___Java edition 1.16.4___
`wget https://launcher.mojang.com/v1/objects/35139deedbd5182953cf1caa23835da59ca3d7cd/server.jar`

A file called `server.jar.1` will be downloaded into the minecraft directory.
Before starting the server, we need to accept the EULA. Do this by running in the command line `java -jar server.jar nogui`. This will generate eula.txt in the same directory. In eula.txt, change `eula=false` to `eula=true` and save the changes.
## Server properties
In the directory where eula.txt was generated, a server.properties file should also have been generated. This file stores settings for a multiplayer server. These settings can be negotiated with club officers. Upon modification of these settings, the server must be restarted from the command line or with the `/reload` command in-game for changes to take effect. 
The server.properties file can be found in this repository.
A reference to all properties can be viewed here: https://minecraft.gamepedia.com/Server.properties
#### Whitelist
In server.properties, there is a field called 'white-list' to enable or disable a whitelist. If this is enabled, the file whitelist.json is generated. Operators are automatically whitelisted.

## Run server
Given the specifications of the server, and once the EULA has been updated, run the server with
`java -Xmx32G -Xms1024M -jar server.jar nogui`
This command starts the server with 1GB of initial memory and can access up to 32GB. `nogui` is set to remove unnecessary overhead.
Additional (optional) arguments include:
 - forceUpgrade: Forces upgrade on all the chunks, such that the data version of all chunks matches the current server version.
 - safeMode: Loads level with vanilla datapack only
 - world \<String>: The name of the world folder in which the level.dat resides 
 ## Join server
 Users may connect to a server using the provided IP address of the server. Anyone will be able to join, unless a whitelist is enabled, where only whitelisted players will be able to join.
## Server rules for players
As stated to be in the scope of the Software Engineering Club, rules must be provided and enforced on the server. [TBD]
## Operators
Operators are administrators of the Minecraft server. They may perform ops commands in-game or remotely in the server console via RCOM, if this is enabled in the server properties. [the password to connect to RCOM console is saved in server.properties]
## Add-ons 
Add-ons are mods, resource packs, and data packs for the base game. The inclusion for add-ons can be negotiates with operators, club officers and the club members who use the server.

# Spigot server
The requirements provided by the club officers requires the instalation of a spigot server. The club officers would like to install spigot plugins such as World Gaurd, chest lock, etc.
## Server setup
Create a directory other than the Minecraft directory. At this step, open JDK should already be installed. download the spigot build tools to the new directory:
`wget https://hub.spigotmc.org/jenkins/job/BuildTools/lastSuccessfulBuild/artifact/target/BuildTools.jar`

Start the build tools script to create the server files. The server files (.jar) will be created in the file location you are at right now:

`git config --global --unset core.autocrlf`, then:
`java -jar BuildTools.jar`

The server files start to build. Copy the server files to the Minecraft directory, such as `/etc/minecraftserver`.
`cp spigot.jar /etc/minecraftserver/`

Navigate to the minecraft server directory. Create this start up script and run it to generate everything needed for the server. In the server directory:
`nano start.sh`
Paster the following code in the text editor:
`
while true; do echo "Starting server now!";
java -Xmx32G -Xms1024M -XX:+UseConcMarkSweepGC -jar spigot.jar nogui
echo "Server restarting in 5 seconds! Press control+c to stop!"; sleep 5; done;
`
The parameter -XX:+UseConcMarkSweepGC defines the garbage collection to operate on the heap.
Save the file and exit.
Run minecraft for the first time using `./start.sh` and accept the EULA agreement as stated above.
Running the new spigot server is now equivalent to running a normal minecraft server. Make sure to run `screen ./start.sh` or the server wll shut down when you close your ssh connection. If a new ssh connection is started, run the following command to open the running server: `screen -r`. Run `stop` to stop the server.

## Adding new plugins
After the club officers have approved the inclusion of a new spigot or bukkit plugin, the plugin will have to be loaded into the server. To do this, move the plugin, which is a .JAR file, into the plugin folder located in the server directory. This folder should automatically be generated upon the first run of the server. Plugins can be added while the server is running. For plugins to take effect, the server must be stopped from the console. After rerunning the server, configuration files, if any, are generated. To apply the effects of the config, use the command `/reload`.
To update plugins without stopping the server, create a folder called `update` in the plugins directory. Download the plugin you want to update; make sure the file name matches the name of the plugin in the plugins directory. Run `/reload` in game to make the following plugin updates.

## Reference:
https://www.vpsserver.com/community/tutorials/4005/minecraft-spigot-bukkit-server-on-ubuntu/
