# Patterns

## access_log

**Example**:    
5.101.0.209 - - [02/Feb/2020:04:22:32 -0500] "POST /vendor/phpunit/phpunit/src/Util/PHP/eval-stdin.php HTTP/1.1" 404 248 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.108 Safari/537.36"    
185.156.177.234 - - [02/Feb/2020:06:02:00 -0500] "\x03" 400 226 "-" "-"     
177.137.119.138 - - [02/Feb/2020:09:06:20 -0500] "-" 408 - "-" "-"        
177.137.119.138 - - [02/Feb/2020:09:11:40 -0500] "-" 408 - "-" "-"     
     
**Match**:    
'%{COMBINEDAPACHELOG}' 

## apache

**Example**:    
[Sun Dec 04 04:51:14 2005] [notice] workerEnv.init() ok /etc/httpd/conf/workers2.properties     
[Sun Dec 04 04:51:14 2005] [notice] workerEnv.init() ok /etc/httpd/conf/workers2.properties     
[Sun Dec 04 04:51:14 2005] [notice] workerEnv.init() ok /etc/httpd/conf/workers2.properties    
[Sun Dec 04 04:51:18 2005] [error] mod_jk child workerEnv in error state 6     
     
**Match**:    
"%{SYSLOG5424SD:timestamp} %{DATA:msg} %{GREEDYDATA:full_msg}"


## cron

**Example**:    
Mar  1 03:11:03 ns508880 run-parts(/etc/cron.daily)[13453]: finished man-db.cron    
Mar  1 03:11:03 ns508880 anacron[13036]: Job `cron.daily' terminated     
Mar  1 03:11:03 ns508880 anacron[13036]: Normal exit (1 job run)     
Mar  1 03:20:01 ns508880 CROND[13861]: (root) CMD (/usr/lib64/sa/sa1 1 1)     
     
**Match**:    
'%{SYSLOGTIMESTAMP:timestamp} %{SYSLOGHOST:machine} %{DATA:application}(?:\[%{POSINT:appid}\])?: %{GREEDYDATA:message}'


## kernel_git

**Example**:    
1487807211	f201ebd87652cf1519792f8662bb3f862c76aa33	-8	mm/z3fold.c	7	3	mm/z3fold.c: limit first_num to the actual range of possible buddy indexes	0     
1487807208	083fb8edda0487d192e8c117f625563b920cf7a4	-8	include/linux/pagemap.h	0	1	mm: fix <linux/pagemap.h> stray kernel-doc notation	1      
1487807205	c87d1655c29500b459fb135258a93f8309ada9c7	-8	Documentation/ABI/obsolete/sysfs-block-zram	0	119	zram: remove obsolete sysfs attrs	2     
1487807205	c87d1655c29500b459fb135258a93f8309ada9c7	-8	Documentation/ABI/testing/sysfs-block-zram	8	92	zram: remove obsolete sysfs attrs	2     
     
**Match**:    
'%{NUMBER:author}\s%{DATA:commit_hash}\s%{NUMBER:commit_utc}\s%{DATA:filename}\s%{NUMBER:additions}\s%{NUMBER:deletions}\s%{GREEDYDATA:subject}\t%{NUMBER:authorID}' 


## web_log

**Example**:    
10.128.2.1	[29/Nov/2017:06:58:55]	GET /login.php HTTP/1.1	200      
10.128.2.1	[29/Nov/2017:06:59:02]	POST /process.php HTTP/1.1	302      
10.128.2.1	[29/Nov/2017:06:59:03]	GET /home.php HTTP/1.1	200      
10.131.2.1	[29/Nov/2017:06:59:04]	GET /js/vendor/moment.min.js HTTP/1.1	200        
     
**Match**:    
'^%{IPORHOST:IP}\t\[%{MONTHDAY}/%{MONTH}/%{YEAR}:%{TIME}]\t%{WORD:act}\s%{NOTSPACE:url}\s%{NOTSPACE:request}\s%{NUMBER:status}' 


## dmesg

**Example**:    
[    0.000000] x86/fpu: Enabled xstate features 0x7, context size is 832 bytes, using 'standard' format.    
[    0.000000] BIOS-provided physical RAM map:     
[    0.000000] BIOS-e820: [mem 0x0000000000000000-0x000000000009d7ff] usable         
[    0.000000] BIOS-e820: [mem 0x000000000009d800-0x000000000009ffff] reserved       
     
**Match**:    
'%{SYSLOG5424SD:sec} %{GREEDYDATA:full_msg}'


## error_log

**Example**:    
[Sun Feb 16 04:54:31.287509 2020] [autoindex:error] [pid 25695] [client 93.42.162.150:37562] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html,index.php) found, and server-generated directory index forbidden by Options directive        
[Sun Feb 16 05:36:29.640274 2020] [autoindex:error] [pid 25698] [client 188.190.197.47:52385] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html,index.php) found, and server-generated directory index forbidden by Options directive       
[Sun Feb 16 06:37:00.029351 2020] [autoindex:error] [pid 25697] [client 106.75.251.241:46556] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html,index.php) found, and server-generated directory index forbidden by Options directive      
[Sun Feb 16 08:25:16.196353 2020] [autoindex:error] [pid 30068] [client 5.101.0.209:56800] AH01276: Cannot serve directory /var/www/html/: No matching DirectoryIndex (index.html,index.php) found, and server-generated directory index forbidden by Options directive      
     
**Match**:    
'^\[(?<timestamp>%{DAY} %{MONTH} %{MONTHDAY} %{TIME} %{YEAR})\]\s+\[%{DATA:descrpt}:%{DATA:descrpt_type}]\s+\[pid\s%{NUMBER:pid}]\s*(?:|\[client\s%{IPORHOST:clientIP}:%{NUMBER:clientID}])\s%{DATA:msg}:\s%{GREEDYDATA:full_msg}' 


## maillog

**Example**:    
Mar  1 10:08:18 CentOS-65-64-minimal postfix/local[2723]: 56E772000112: to=<CentOS-25-64-minimal.localdomain>, orig_to=<root>, relay=local, delay=72497, delays=72497/0.04/0/0.06, dsn=2.0.0, status=sent (delivered to mailbox)      
Mar  1 10:08:18 CentOS-65-64-minimal postfix/qmgr[8847]: 56E772000112: removed      
Mar  1 14:30:02 CentOS-65-64-minimal postfix/pickup[18699]: 04D4A200013A: uid=0 from=<root>     
Mar  1 14:30:02 CentOS-65-64-minimal postfix/cleanup[26034]: 04D4A200013A: message-id=<20200301123002.CentOS-2-64-minimal.localdomain>      
     
**Match**:    
'%{SYSLOGTIMESTAMP:timestamp} %{SYSLOGHOST:machine} %{DATA:msg}: %{DATA:msg_id}: %{GREEDYDATA:full_msg}' 


## messages

**Example**:    
Feb 16 03:43:01 ns508880 rsyslogd: [origin software="rsyslogd" swVersion="8.24.0-41.el7_7.2" x-pid="1159" x-info="http:/rsyslog.com"] rsyslogd was HUPed    
Feb 16 03:50:01 ns508880 systemd: Started Session 10121 of user root.     
Feb 16 04:00:01 ns508880 systemd: Started Session 10122 of user root.      
Feb 16 04:01:01 ns508880 systemd: Started Session 10123 of user root.       
     
**Match**:    
'%{SYSLOGTIMESTAMP:timestamp} %{SYSLOGHOST:machine} %{DATA:application}: %{GREEDYDATA:message}'


## mysqld

**Example**:    
2019-12-12T14:11:14.177038Z 0 [System] [MY-011323] [Server] X Plugin ready for connections. Socket: '/var/run/mysqld/mysqlx.sock' bind-address: '::' port: 33060        
2019-12-12T15:36:48.447916Z 22 [Warning] [MY-010055] [Server] IP address '170.130.187.42' could not be resolved: Name or service not known          
2019-12-12T15:49:39.688127Z 23 [Warning] [MY-010055] [Server] IP address '220.191.254.66' could not be resolved: Name or service not known       
2019-12-12T20:21:04.383578Z 24 [Warning] [MY-010055] [Server] IP address '61.153.237.123' could not be resolved: Name or service not known             
     
**Match**:    
'%{TIMESTAMP_ISO8601:timestamp} %{NUMBER:threadid} \[%{DATA:priority}] \[%{DATA:err_code}] \[%{DATA:subsystem}] %{GREEDYDATA:message}'


## secure

**Example**:    
Feb 16 03:43:27 ns508880 sshd[25726]: Failed password for root from 49.88.112.112 port 47108 ssh2           
Feb 16 03:43:28 ns508880 sshd[25726]: pam_succeed_if(sshd:auth): requirement "uid >= 1000" not met by user "root"           
Feb 16 03:43:29 ns508880 sshd[25726]: Failed password for root from 49.88.112.112 port 47108 ssh2         
Feb 16 03:43:30 ns508880 sshd[25726]: Received disconnect from 49.88.112.112 port 47108:11:  [preauth]            
     
**Match**:    
'%{SYSLOGTIMESTAMP:timestamp} %{SYSLOGHOST:machine} %{DATA:application}(?:|\[%{NUMBER:appid}]): %{GREEDYDATA:message}'


## yum

**Example**:    
Feb 24 04:14:38 Installed: python-backports-ssl_match_hostname-3.5.0.1-1.el7.noarch          
Feb 24 04:14:38 Installed: python-setuptools-0.9.8-7.el7.noarch          
Feb 24 04:14:39 Installed: python2-pip-8.1.2-12.el7.noarch           
Feb 25 03:39:54 Updated: firewalld-filesystem-0.6.3-2.el7_7.3.noarch          
     
**Match**:    
'%{SYSLOGTIMESTAMP:timestamp} %{DATA:type}: %{GREEDYDATA:message}'