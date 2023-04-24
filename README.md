# PwnAdventure3

1.	Download Oracle VM VirtualBox or any other software that lets you create VMs
2.	Install Oracle VM VirtualBox
3.	Download Ubuntu 16 server 64-bit iso file
4.	Create a new VM with Ubuntu server and check the ***Skip Unattended Installation***
- 4096 MB Memory
- 2 CPUs
- 25,00 GB Virtual Hard Disk
5.	Press settings with the newly created VM selected and navigate to the Network page
- In the Adapter 1 window change the ***Attached to: NAT*** -> ***Attached to: Bridged Adapter***
6.	Install Ubuntu
- Select your language
- Select “Install Ubuntu Server”
- Select your language and location and locales
- Select your keyboard
- Add a hostname (ubuntu-pwnadventure-server for example)
- Create a user
- Select no for encrypting the home directory
- Select Guided – use entire disk and set up LVM
- Continue without proxy
- Install security updates automatically
7.	Login into your user
8.	Set up SSH
- Write `sudo apt-get install ssh`
- Write `ip a` and write down the IP address for the second entry
- Should look like this: `192.168.x.xxx`
9.	Open CMD on your main computer and connect to the VM via SSH
- Command: `ssh username_for_user@the_IP_above`
- Should look like this: `ssh user@192.168.0.222`
- Add the IP to the list of known hosts
- Type in your password for the user
10.	Navigate to the main directory
- Type `cd ..` twice
11.	Create a directory 
- Command: `sudo mkdir PwnAdventure3`
12.	Navigate into the PwnAdventure3 directory
- Command: `cd PwnAdventure3`
13.	Download and unpack the server files then delete the archive
- Command: `sudo wget http://pwnadventure.com/pwn3.tar.gz`
- Command: `sudo tar -xvf pwn3.tar.gz`
- Command: `sudo rm pwn3.tar.gz -r`
14.	Install postgresql and update/upgrade packages
- Command: `sudo apt-get install postgresql`
- Command: `sudo apt-get update`
- Command: `sudo apt-get upgrade`
15.	Create user without password
- Command: `sudo adduser pwn3 --disabled-password`
16.	Switch to the postgres user and create a template
- Command: `sudo su postgres`
- Command: `psql template1`
17.	Create a user and a database and grant all privileges then quit psql
- Command: `create user pwn3;`
- Command: `create database master;`
- Command: `grant all privileges on database master to pwn3;`
- Command: `\q`
18.	Switch back to your user and login into pwn3
- Command: `exit`
- Command: `sudo su pwn3`
19.	Navigate to the master server directory and initialize the initdb.sql
- Command: `cd PwnAdventure3/server/MasterServer`
- Command: `psql -f initdb.sql -d master -U pwn3`
20.	Create a server account for the MasterServer and save the details
- Command: `./MasterServer --create-server-account`
- Should look like this: `Username: server_b15139a6705a342f and Password: 15482a7bf1e7a7619e303290`
21.	Change user back to your main one
- Command: `exit`
22.	Navigate to the server.ini file and configure the servers
- Command: `cd PwnAdventure3/client/PwnAdventure3_Data/PwnAdventure3/PwnAdventure3/Content/Server`
- Command: `sudo nano server.ini`
- Overwrite the Hostname for the MasterServer to localhost
- Overwrite the Hostname for the GameServer to the IP above
- Overwrite the username and password with the details above
- Save to server.ini file
- Should look like this:
```
[MasterServer]
Hostname=localhost
Port=3333

[GameServer]
Hostname=192.168.0.222
Port=3000
Username=server_b15139a6705a342f
Password=15482a7bf1e7a7619e303290
Instances=
```
23.	Navigate to the MasterServer
- Command: `cd PwnAdventure3/server/MasterServer`
24.	Change permission for the MasterServer
- Command: `chmod +x MasterServer`
25.	Change back to pwn3 user
- Command: `sudo su pwn3`
26.	Start the MasterServer
- Command: `./MasterServer`
27.	Open a new CMD and connect to your VM again
- Command: `ssh username_for_user@the_IP_above`
- Should look like this: `ssh user@192.168.0.222`
- Type in your password for the user
28.	Navigate to the GameServer then change permission for it
- Command: `cd PwnAdventure3/client/PwnAdventure3_Data/PwnAdventure3/PwnAdventure3/Binaries/Linux`
- Command: `chmod +x PwnAdventure3Server`
29.	Start the GameServer
- Command: `./PwnAdventure3Server`
30.	Download client for your OS and navigate to the server.ini
- `~\PwnAdventure3\PwnAdventure3\Content\Server`
31.	Modify the server.ini to look something like this
```
[MasterServer]
Hostname=master.pwn3
Port=3333

[GameServer]
Hostname=game.pwn3
Port=3000
Username=
Password=
Instances=
```
32.	Add the host IP to the hosts file
- On windows it’s in the `C:\Windows\System32\drivers\etc` directory
- Should look like this:
```
#      102.54.94.97     rhino.acme.com          # source server
#       38.25.63.10     x.acme.com              # x client host
192.168.0.222  master.pwn3
192.168.0.222	game.pwn3
```
33.	Start the client and register
34.	Enjoy
