# Simple Adversary Defense Script (SAD Script)

Defend your Linux machine from a simple adversary who has taken down defenses and established persistence!

## About The Project 

SAD Script was developed to emulate common misconfigurations utilized by cyber defense competitions. SAD Script is written in BASH and is simple to understand and fast to run.

After SAD Script has run, it's your job to identify what has changed in your system and how to remediate the issues that have been created. Try to kick the hacker out, keep them out, discover how they established perisistance and what changes they made to your system that make it vulnerable to future attacks.


## Getting Started 

SAD Script is easy!

### Prerequisites

SAD Script has only been tested on Ubuntu 22.x.x and Debian 10+ but should work on systems based on systemd that have the following:
- Debian
- User must have all sudo perms 
- python3

It is recommended to run SAD Script on a Debian 10+ Virtual Machine. Remember to take a snapshot BEFORE running SAD Script as your VM will be abused. 


### Installation 

Clone the repo into any directory you'd like. 
```
git clone https://github.com/Jacob-Ham/SADScript.git
```

Enter the directory
```
cd SADScript
```
## Usage 

Using SAD Script is as simple as executing the bash script with sudo. 

```
sudo bash SADScript.sh
```

Now just wait, SAD Script is abusing your system and installing required packages. This usually takes about 1 minute but can take longer depending on your system and internet speed.

## Script Flow

The script performs the following actions:

1.  Calls the `CreateTaunt` function to print a taunting message to the console.
2.  Calls the `CheckConfig` function to check the configuration settings in the `config.conf` file.
3.  Based on the configuration settings, either:
    -   Creates the environment silently, i.e., without any output to the console, by running the `Run` function with output redirected to `/dev/null`.  
    -   Creates the environment with normal output to the console by running the `Run` function.
4.  The `Run` function calls a series of sub-functions that create various security issues and configurations, including:
    -   `CreateEnvironment`: Updates the system and installs various tools and packages.
    -   `CreateConfigs`: Creates configurations related to SSH and bash shell.
    -   `CreatePersistence`: Creates services and cron jobs to demonstrate attacker persistence.
    -   `CreateBadUsers`: Creates a ton of new users.
    -   `CreatePrivEsc`: Creates vulnerabilities that can be exploited for privilege escalation.
    -   `CreateExposures`: Creates vulnerabilities that can be exploited for remote access.
    -   `CreateLogins`: Creates a hacker user with SSH access to the system.
5.  Each sub-function checks the configuration settings in `config.conf` to determine whether to create the vulnerability or skip it.
6.  The `CreateSSHLogin` function creates a persistent SSH connection to the hacked system that is used by the hacker to maintain access and perform further attacks.


## What does SAD Script Cover? 


-  Creates Environment:
	  - Installs cowsay
	  - Installs sshpass
	  - Installs openssh-server
	  - Installs sl
	  - Installs cmatrix
	  - Removes curl
-  Change the Users .bashrc aliases to:
	- alias ls="sl -a"
	- alias cd="cowsay You may not cd"
	- alias cat="echo meow"
	- alias nano="echo nanope"
	- alias su="echo Super User Dont"
	- alias sudo="cmatrix"
	- Teaching absolute paths and terminal resets
- Spawn fake systemd services 
	- Creates directory /root/.script/
	- Creates directory /root/.util/
	- Creates two scripts for the services to run (script.sh, net.sh)
	- script.sh just echo text
	- net.sh creates new hacker users, gives them bash shell, and adds them to sudoers every 5 min
	- Creates evilservice.service and sysnet.service in /etc/systemd/system/
	- evilservice.service - Easy to find using systemctl 
	- sysnet.service - More difficult to find using systemctl
-  Creates 69 random users with /bin/sh shells and random passwords 
	- Should teach bash oneliner to disable accounts or change login shells
- Changes SSH Config 
	- Disables StrictHostKeyChecking 
	- Enables root login in sshd_config
- Creates a user spawn script 
	- Located in /root/.script/spawn.sh 
	- Creates 4 new users every time it is ran. 
- Makes root Cronjob 
	- Cronjob will run every minute 
	- Calls /root/.script/spawn.sh to spawn users. 
- Creates one root level account with /bin/bash shell
	- This user is logged into to an ssh shell every 10 seconds if kicked out 
	- Used to teach checking logged in users and disabling accounts. 
- Creates a python web server on port 80
	- Web server exposes files on system. 
- Creates two SUID perm binaries that give root shell when run (shoutout Jack W for the uber small binary). 
	- One located in /opt/scripts/shell - easy to catch. 
	- One located in /usr/bin/curl - more difficult to catch
- Root "hacker" users now have a .bash_history file
	- Supposed to give a clue as to how they priv esc with the fake curl binary

### To-Do
- Disable more SSH configs: key auth
- Create another service that re-runs the SAD Script at an interval and reintroduce vulns. Establishes attackers persistance. 
- Have the emulated "hacker" run commands and steal data. 
- Modify executables 
- Installs and enables UFW
	- Allows all incoming and outgoing to the default rules
- Spawn reverse shell 
