# Linux Checklist
**Cyberpatriots personal checklist**


## Checklist

1. Read the README

	Note down which ports/users are allowed.
        
1. **Answer forensic questions**

      If you need to find files use the command “find /home -name '*put in here’' -type f” You can change “/home” to “/” if you want to search the entire computer.

1. View System Logs by typing in `System Log` in Ubuntu's Search Feature
     
        * Four types of logs
        * auth.log: Tracks authentication events that prompt for user passwords (e.g., uses of PAM files and sudo)
        * dpkg.log: Tracks software events (e.g., installations and updates)
        * syslog: Tracks operating system events (e.g. error messages)
        * Xorg.0.log: Tracks desktop events (e.g., service changes and graphic card errors
        
1. Enable Audits
	* sudo apt install auditd
	* Enable audits by typing auditctl –e 1
	* View and modify policies in /etc/audit/auditd.conf

1. Run LinuxScript
        * Inspect `~/Desktop/Script.log`
        
1. Manage users. 
    * Delete any that aren’t supposed to exist. 
    * Undisable the accounts that are supposed to exist. 
    * Make sure everyone who should be admin is admin and everyone who is supposed to be standard is standard. 
    * Add any that are needed. Make sure to unlock and re-lock. System Settings> Users and Groups > Unlock. 
    ```
        * Check /etc/sudoers.d and make sure only members of group sudo can sudo.
        * Check /etc/group and remove non-admins from sudo and admin groups.
    ```

1. Change the name of the admin account

1. PermitRootLogin to no. You might need to stop ssh, edit config, and restart.
    ```
    * Sudo nano /etc/ssh/sshd_config
    * Sudo service ssh restart
    * OR passwd -l root
    ```

1. Look in the README for “insecure” passwords. Change those users’ passwords.
    * `sudo chpasswd` is very useful for this purpose. (`user:password`)
    
1. **System Settings>Software&Updates** have it check for recommended updates once a day. (check important and recommended)

1. **Delete all non-work related files** (If specified in readme) use:
    *  `find / -name '*.<file extension>' -type f -delete. `
    
        `Remove .mp3, .mov, .mp4, .avi, .mpg, .mpeg, .flac, .m4a, .flv, .ogg, .gif, .png, .jpg, and .jpeg.`
    * Check user directories.
        * `cd /home`
        * `sudo ls -Ra`
        * `rm -i /home/etc`  -remove files
        * `rm -v /home/vivek/data/*`      - removes all files in data
        
1. Enable Firewall (*Allow any ports in the README*): `sudo ufw enable`
    * install gufw
    * Deny all incoming traffic and allow outgoing traffic
    
1. Check for any bad sources: `Sudo gedit /etc/apt/sources.list`

1. Check for any bad sources: `Sudo gedit /etc/hosts`

1. Check rc.local: `Sudo gedit /etc/rc.local`
 	* Should be empty except `exit 0`
	
1. Check crontab: Check for anything in there, it might be malicious
	* Check CIS
	* put hashtags on all things
	* Remove startup tasks
	* `Sudo ls /var/spool/cron/crontabs` - checks for who has it
	* Allow only root in cron
		* (could do) `gedit /etc/cron.allow and /etc/cron.deny`
		
1. Stop Cookies: `sysctl -n net.ipv4.tcp_syncookies`

1. Disable IPv6 (Potentially harmful)
	* `echo "net.ipv6.conf.all.disable_ipv6 = 1" | sudo tee -a /etc/sysctl.conf`
	
1. Disable IP Forwarding
	* `echo 0 | sudo tee /proc/sys/net/ipv4/ip_forward`

1. Prevent IP Spoofing
	* `echo "nospoof on" | sudo tee -a /etc/host.conf`
	
1. Turn off Guest Session: 
	* `Sudo nano /etc/lightdm/lightdm.conf` - `allow-guest=false`
	* Then do `sudo service lightdm restart` *(make sure you aren’t doing any updates when you restart lightdm)*

1. Remove hacking tools. **CHANGE BY CHECKING PRESENTATION**
	*  Open Ubuntu Software center and look at recently installed software for “nmap”, “ophcrack”, or anything else that looks suspicious. If in doubt look up its name.
	* (these may not be their actual names in software repositories): `nmap`, `kismet`, `netcat`, `John the Ripper`, `Hydra`, `Aircrack-NG`, `FCrackZIP`, `LCrack`, `OphCrack`, `PDFCrack`, `Pyrit`, `RARCrack`, `SipCrack`, `IRPAS`, `LogKeys`, `Zeitgeist`, `NFS`, `NGINX`, `Inetd`, `VNC`, and `SNMP`, Wireshark
	*  `sudo apt list --installed | grep ‘<name>\|<name>|\<name>’`
	*  **CHANGE** `Sudo apt-get remove <package>`
	* *(Optional)* Install synaptic (type in installed)
	*  `grep “install” /var/log/apt/history.log`
	
1. **Remove non-work related software.** Anything that looks like a game should be removed. If in doubt look it up. If you find a file called “passwords.txt” make sure to delete it.
	* **CHANGE** sudo apt-get --purge remove {program}
	* `Rm -i /home/etc`      - removes files (use quotes for spaces)
	* `rm -v /home/vivek/data/*`      - removes all files in data

1. Update System
	* Go to terminal and “sudo apt-get update” and then “sudo apt-get upgrade” and “sudo apt-get dist-upgrade”.  Let the apps update while you are doing other stuff.
	* Ensure that you have points for upgrading the kernel, each service specified in the readme, and bash if it is vulnerable to shellshock.
	* (maybe) `sudo apt autoremove` 

1. Firefox
```
	* Update it
	* Open FireFox; click on the three lines on the top right of the browser; select the menu button on the top right to enter preferences; select the content tab on the left-hand side of the window; check the box next to Block pop-up windows
	* block dangerous and deceptive content
	* Warns when sites try to install add-ons
	* Blocks dangerous downloads
	* Automatically checks for updates
	* Look for any more
```

1. Enforce Password Requirements.
	* Add a minimum password length, password history, and add complexity requirements.
		* Open `/etc/pam.d/common-password` with sudo.
		* Add `minlen=8` to the end of the line that has `pam_unix.so` in it.
		* Add `remember=5` to the end of the line that has `pam_unix.so` in it.
		* Locate the line that has pam.cracklib.so in it. If you cannot find that line, install cracklib with `sudo apt-get install libpam-cracklib`.
		* Add `ucredit=-1 lcredit=-1 dcredit=-1 ocredit=-` to the end of that line.
		* Implement an account lockout policy.
			* Open `/etc/pam.d/common-auth`.
			* Add `deny=5 unlock_time=1800` to the end of the line with `pam_tally2.so` in it.
			* *(if necessary)* Add “auth required pam_tally2.so deny=5 onerr=fail unlock_time=1800” (all on one line) to the end of the file. This denies password attempts and adds a lockout period.
	* “Sudo nano /etc/login.defs” change/add to:
```		* PASS_MAX_DAYS 90
		* PASS_MIN_DAYS 7
		* PASS_WARN_AGE 14
```

1. Audit Services
	* `Sudo apt-get install bum` - Use bum to look for bad services. Type “sudo bum” to start bum.
	* Remove apache, nginx, bind9 (DNS), ssh, FTP, bluetooth, maybe cups unless otherwise stated in the README. (check CIS “not enabled”)
	
1. Disable ftp
	* dpkg - l | grep ftp will tell you what ftp package daemon is running, so you know what package to remove
	* Typically it will be pure-ftp, remove it with sudo apt remove pure-ftpd
	
1. Disable samba
	*  (unless readme says otherwise) using `Sudo service smbd stop` and `Sudo service samba stop` (also uninstall sasumba too)

1. *(Hard)* Make sure only the default account can sudo.: `sudo visudo`

1. Purge netcat. Use `sudo apt-get purge netcat nc netcat-*` to purge all forms of netcat

1. Secure Ports. Follow these steps:
	* `Sudo netstat -pnltu` - lists all ports with names
	* `Sudo ss -ln | grep tcp` - This lists all open ports
	* Look at the list of open ports and use `sudo lsof - i :<Port>` to get the program
	* Determine if the port is a backdoor (if it has nc or netcat in the name it is a backdoor)
	* Determine if the program is supposed to be on the computer
	* Copy the program which is listening on the port. `whereis $program`
	* Copy where the program is (if there is more than one location, just copy the first one). `dpkg -S $location`
	* This shows which package provides the file (If there is no package, that means you can probably delete it with `rm $location`; `killall -9 $program`). `sudo apt-get purge $package`
	* Check to make sure you aren't accidentally removing critical packages before hitting "y".
	* `sudo ss -l` to make sure the port actually closed.
	* These ports are safe: 22, 53, 631, 35509
	* Another way to close a port is: `sudo ufw deny 8080`
	
1. Unhide (Shows Hidden Processes & TCP/UDP Ports)
	* `sudo unhide proc sys brute
	* `sudo unhide-tcp

1. Correct file permissions: Execute the following commands to put correct file permissions on important system files (with sudo): 
	* `chmod -R 444 /var/log`
	* `chmod 440 /etc/passwd`
	* `chmod 440 /etc/shadow`
	* `chmod 440 /etc/group`
	* `chmod -R 444 /etc/ssh`

1. Run chkrootkit, rkhunter, clam, lynis
	* (REMINDERS) SSH CHECK LOGS `/var/log/auth.log | grep -i failed NOT`

1. Check all files with a file permission of 700-777
	* `find / -type f -perm 777`
 
1. Check the PHP scripts
	* `Find / -name ‘*.php” -type f`

## Useful Information

1. Switch users in terminal
	* `sudo su - username`
	
1. To check if you are logged into sshd
	* `ps -o comm= -p $PPID`

1. Add users to Group 
	* To list all groups: cat /etc/group  
	* To add a group: `addgroup [groupname]`
	* To add a user to a group: `adduser [username] [groupname]`
	* To delete a group: `sudo groupdel {group-name-here}`
	* To delete a user: `sudo userdel -r $user`
	* To delete a user from a group: `sudo deluser {user} {group}`

1. Antivirus
	* `Apt-get search clam` - searches for them
	* `apt-cache search rootkit` - searches for them
	* `Sudo apt-get install calmav clamtk rkhunter chkrootkit` - downloads them
	* `Sudo freshclam` - grabs the latest version
	* `Sudo clamscan -ir /home/`
	* `Sudo chkrootkit > rookitresults.txt`
	* `Sudo chkrootkit -q`
	* `Sudo rkhunter -c`
	* `Sudo grep -i warning /var/log/rkhunter.log`

1. HardInfo (Hardware Analysis, System Benchmark, & Report Generator)
	* `hardinfo -r -f html`
1. Lynis (Security Auditing).
	* `Lynis update`
	* `Sudo lynis audit system`
	* Apt-listbugs, needrestart, debsecan, debsums - update some of these
1. Unhide (Shows Hidden Processes & TCP/UDP Ports)
1. Nmap (Network mapper) -> DELETE AFTER USE





