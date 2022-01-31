# Bash Logging for Centos or Ubuntu

## 1-) Centos

### Ignore space Close

	sudo export HISTCONTROL=""

### Edit /etc/bashrc

	sudo -e /etc/bashrc    				//add the following line to the end of the file
	
	export PROMPT_COMMAND='command_status=$?; command_path=$PWD; last_command=$(history -a >(tee -a $HISTFILE)); if [[ -n "$last_command" ]]; then logger -p local6.debug "$(whoami) [$$]: Path = $command_path commandLine = $(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//" ) commandStatus=$command_status"; history -c; history -r; fi'


### Run the following to load the changes

	source /etc/bashrc

### Edit /etc/rsyslog.d/bash.conf

	sudo -e /etc/rsyslog.d/bash.conf  		//add the following line to the beginning of the file
	local6.*    /var/log/commands.log

### Edit /etc/logrotate.d/syslog

	sudo -e /etc/logrotate.d/syslog 		//add the following line to the beginning of the file
	/var/log/commands.log

### Edit /etc/rsyslog.conf
	
	sudo vi /etc/rsyslog.conf			//add the following line to the end of the file
	local6.* @@{destinationIP}:{destinationPort}

### Restart rsyslog service

	sudo service rsyslog restart
	

Note ==> @@ : tcp | @ : udp 


## 2-) Ubuntu

### Ignore space Close

	sudo export HISTCONTROL=""

### Edit /etc/bash.bashrc

	sudo -e /etc/bashrc    				//add the following line to the end of the file
	
	export PROMPT_COMMAND='command_status=$?; command_path=$PWD; last_command=$(history -a >(tee -a $HISTFILE)); if [[ -n "$last_command" ]]; then logger -p local6.debug "$(whoami) [$$]: Path = $command_path commandLine = $(history 1 | sed "s/^[ ]*[0-9]\+[ ]*//" ) commandStatus=$command_status"; history -c; history -r; fi'

### Run the following to load the changes

	source /etc/bash.bashrc

### Edit /etc/rsyslog.d/bash.conf

	sudo -e /etc/rsyslog.d/bash.conf  		//add the following line to the beginning of the file
	local6.*    /var/log/commands.log

### Edit /etc/logrotate.d/syslog

	sudo -e /etc/logrotate.d/rsyslog 		//add the following line to the beginning of the file
	/var/log/commands.log

### Edit /etc/rsyslog.conf
	
	sudo vi /etc/rsyslog.conf			//add the following line to the end of the file
	local6.* @@{destinationIP}:{destinationPort}

### Restart rsyslog service

	sudo service rsyslog restart

Note ==> @@ : tcp | @ : udp 
