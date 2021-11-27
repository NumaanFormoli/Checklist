# Collection of Checklists

1. Changed Update
  - sudo apt-get update
  - sudo apt-get upgrade
  - sudo apt-get autoremove
  - sudo apt-get autoclean

Enable automatic updates can be crucial for your server security. It is very important to stay up to date.

    sudo apt-get install unattended-upgrades
    sudo dpkg-reconfigure -plow unattended-upgrades

To enable ONLY security updates, please change the code to look like this:

    sudo nano /etc/apt/apt.conf.d/50unattended-upgrades
    : Unattended-Upgrade::Allowed-Origins {
    :     "${distro_id}:${distro_codename}-security";
    : //  "${distro_id}:${distro_codename}-updates";
    : //  "${distro_id}:${distro_codename}-proposed";
    : //  "${distro_id}:${distro_codename}-backports";
    : };
    : // Unattended-Upgrade::Mail "my_user@my_domain.com";



1. Add Swap

1. Configure sysctl.conf

"/etc/sysctl.conf" file is used to configure kernel parameters at runtime. Linux reads and applies settings from this file.

 sudo nano /etc/sysctl.conf

    # IP Spoofing protection
    : net.ipv4.conf.default.rp_filter = 1
    : net.ipv4.conf.all.rp_filter = 1
    # Block SYN attacks
    : net.ipv4.tcp_syncookies = 1
    # Controls IP packet forwarding
    : net.ipv4.ip_forward = 0
    # Ignore ICMP redirects
    : net.ipv4.conf.all.accept_redirects = 0
    : net.ipv6.conf.all.accept_redirects = 0
    : net.ipv4.conf.default.accept_redirects = 0
    : net.ipv6.conf.default.accept_redirects = 0
    # Ignore send redirects
    : net.ipv4.conf.all.send_redirects = 0
    : net.ipv4.conf.default.send_redirects = 0
    # Disable source packet routing
    : net.ipv4.conf.all.accept_source_route = 0
    : net.ipv6.conf.all.accept_source_route = 0
    : net.ipv4.conf.default.accept_source_route = 0
    : net.ipv6.conf.default.accept_source_route = 0
    # Log Martians
    : net.ipv4.conf.all.log_martians = 1
    # Block SYN attacks
    : net.ipv4.tcp_max_syn_backlog = 2048
    : net.ipv4.tcp_synack_retries = 2
    : net.ipv4.tcp_syn_retries = 5
    # Log Martians
    : net.ipv4.icmp_ignore_bogus_error_responses = 1
    # Ignore ICMP broadcast requests
    : net.ipv4.icmp_echo_ignore_broadcasts = 1
    # Ignore Directed pings
    : net.ipv4.icmp_echo_ignore_all = 1
    : kernel.exec-shield = 1
    : kernel.randomize_va_space = 1
    # disable IPv6 if required (IPv6 might caus issues with the Internet connection being slow)
    : net.ipv6.conf.all.disable_ipv6 = 1
    : net.ipv6.conf.default.disable_ipv6 = 1
    : net.ipv6.conf.lo.disable_ipv6 = 1
    # Accept Redirects? No, this is not router
    : net.ipv4.conf.all.secure_redirects = 0
    # Log packets with impossible addresses to kernel log? yes
    : net.ipv4.conf.default.secure_redirects = 0
    
    # [IPv6] Number of Router Solicitations to send until assuming no routers are present.
    # This is host and not router.
    : net.ipv6.conf.default.router_solicitations = 0
    # Accept Router Preference in RA?
    : net.ipv6.conf.default.accept_ra_rtr_pref = 0
    # Learn prefix information in router advertisement.
    : net.ipv6.conf.default.accept_ra_pinfo = 0
    # Setting controls whether the system will accept Hop Limit settings from a router advertisement.
    : net.ipv6.conf.default.accept_ra_defrtr = 0
    # Router advertisements can cause the system to assign a global unicast address to an interface.
    : net.ipv6.conf.default.autoconf = 0
    # How many neighbor solicitations to send out per address?
    : net.ipv6.conf.default.dad_transmits = 0
    # How many global unicast IPv6 addresses can be assigned to each interface?
    : net.ipv6.conf.default.max_addresses = 1
    
    # In rare occasions, it may be beneficial to reboot your server reboot if it runs out of memory.
    # This simple solution can avoid you hours of down time. The vm.panic_on_oom=1 line enables panic
    # on OOM; the kernel.panic=10 line tells the kernel to reboot ten seconds after panicking.
    : vm.panic_on_oom = 1
    : kernel.panic = 10

    # Apply new settings
    sudo sysctl -p

1. Disable IRQ Balance

        sudo nano /etc/default/irqbalance
        : ENABLED="0"

1. OpenSSL Heartbleed Bug
- It is crutial to fix this issue to version greater or equal to 1.0.1g. You also have to revoke and regenerate new keys and certificates and re-issuing of CA certs and the like in the coming days.
    
        openssl version -v
    
        # above should be not 1.0.1f or below, otherwise:
        sudo apt-get update
        sudo apt-get upgrade openssl libssl-dev
        apt-cache policy openssl libssl-dev
    
        sudo apt-get install make
        curl https://www.openssl.org/source/openssl-1.0.2f.tar.gz | tar xz && cd openssl-1.0.2f && sudo ./config && sudo make && sudo make install
        sudo ln -sf /usr/local/ssl/bin/openssl `which openssl`
    
        openssl version
    
4. remove auto login

5. Secure `tmp` and `/var/tmp`

6. SSH Set Up and Hardening
    - http://bookofzeus.com/harden-ubuntu/hardening/ssh/
    - IMPORTANT: IF you change your ssh port, make sure you add the rule in the iptables.
    - 
          sudo nano /etc/ssh/sshd_config
          : Port <port>
          : Protocol 2
          : LogLevel VERBOSE
          : PermitRootLogin no
          : StrictModes yes
          : RSAAuthentication yes
          : IgnoreRhosts yes
          : RhostsAuthentication no
          : RhostsRSAAuthentication no
          : PermitEmptyPasswords no
          : PasswordAuthentication no
          : ClientAliveInterval 300
          : ClientAliveCountMax 0
          : AllowTcpForwarding no
          : X11Forwarding no
          : UseDNS no

          sudo nano /etc/pam.d/sshd	(comment lines below)
          : #session	optional	pam_motd.so motd=/run/motd.dynamic noupdate
          : #session	optional	pam_motd.so # [1]

          sudo service ssh restart
 
7. Secure Shared Memory
  - http://bookofzeus.com/harden-ubuntu/server-setup/set-hostname-and-host/
 
          sudo nano /etc/fstab
          tmpfs	/run/shm	tmpfs	ro,noexec,nosuid	0 0
  
    - If you want to make the changes without rebooting, you can run:
           
           sudo mount -a
  
8. Set Hostname and Host File
  
       sudo nano /etc/hostname
       : <ip/hostname>
    
       sudo nano /etc/hosts
       : 127.0.0.1	localhost localhost.localdomain <ip/hostname>
  
9. Set Locale and Timezone

        sudo locale-gen en_GB.UTF-8
        sudo update-locale LANG=en_GB.UTF-8
        sudo dpkg-reconfigure tzdata

10. Set Security Limits

        sudo nano /etc/security/limits.conf
        : user1 hard nproc 100
        : @group1 hard nproc 20
  
11. IP Spoofing

    sudo nano /etc/host.conf
    : order bind,hosts
    : nospoof on
  
12. PHP
- http://bookofzeus.com/harden-ubuntu/hardening/php/

        sudo nano /etc/php/fpm/php.ini
        : safe_mode = On
        : safe_mode_gid = On
        : sql.safe_mode = On

        : register_globals = Off
        : magic_quotes_gpc = Off

        : expose_php = Off
        : track_errors = Off
        : html_errors = Off
        : display_errors = Off

        : disable_functions = ... system,exec,shell_exec,php_uname,getmyuid,getmypid,leak,listen,diskfreespace,link,ignore_user_abord,dl,set_time_limit,highlight_file,source,show_source,passthru,fpaththru,virtual,posix_ctermid,posix_getcwd,posix_getegid,posix_geteuid,posix_getgid,posix_getgrgid,posix_getgrnam,posix_getgroups,posix_getlogin,posix_getpgid,posix_getpgrp,posix_getpid,posix,_getppid,posix_getpwnam,posix_getpwuid,posix_getrlimit,posix_getsid,posix_getuid,posix_isatty,posix_kill,posix_mkfifo,posix_setegid,posix_seteuid,posix_setgid,posix_setpgid,posix_setsid,posix_setuid,posix_times,posix_ttyname,posix_uname,proc_open,proc_close,proc_get_status,proc_nice,proc_terminate,phpinfo
        # exceptions: getmypid

        : allow_url_fopen = Off
        : allow_url_include = Off

        : sql.safe_mode = On

        : session.cookie_httponly = 1
        : session.referer_check = mydomain.com
 
