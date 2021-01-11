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
        
1. Allow any ports in the README: `sudo ufw enable`
    * install gufw
    * Deny all incoming traffic and allow outgoing traffic
    
1. 
