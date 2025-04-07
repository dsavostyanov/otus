# Systemd — создание unit-файла
```
root@otus1:~# touch /etc/default/watchlog /var/log/watchlog.log /opt/watchlog.sh /etc/systemd/system/watchlog.service /etc/systemd/system/watchlog.timer && chmod +x /opt/watchlog.sh

/etc/default/watchlog:
# Configuration file for my watchlog service
# Place it to /etc/default

# File and word in that file that we will be monit
WORD="ALERT"
LOG="/var/log/watchlog.log"

/var/log/watchlog.log:
dfs fsf alert
asdasd ds  dfsf
ALERT vv ll
asad ALERT km

/opt/watchlog.sh
#!/bin/bash

# Source variables from environment file
. /etc/default/watchlog

DATE=$(date)

if grep -q "$WORD" "$LOG"; then
    logger "$DATE: I found the word '$WORD', Master!"
else
    exit 0
fi


/etc/systemd/system/watchlog.service
[Unit]
Description=My watchlog service

[Service]
Type=oneshot
ExecStart=/opt/watchlog.sh

/etc/systemd/system/watchlog.timer
[Unit]
Description=Run watchlog script every 30 second

[Timer]
OnUnitActiveSec=30
Unit=watchlog.service

[Install]
WantedBy=multi-user.target

root@otus1:~# systemctl status watchlog.timer
● watchlog.timer - Run watchlog script every 30 second
     Loaded: loaded (/etc/systemd/system/watchlog.timer; disabled; preset: enabled)
     Active: active (waiting) since Fri 2025-04-04 15:03:22 UTC; 3min 50s ago
    Trigger: Fri 2025-04-04 15:07:38 UTC; 25s left
   Triggers: ● watchlog.service

Apr 04 15:03:22 otus1 systemd[1]: Stopped watchlog.timer - Run watchlog script every 30 second.
Apr 04 15:03:22 otus1 systemd[1]: Stopping watchlog.timer - Run watchlog script every 30 second...
Apr 04 15:03:22 otus1 systemd[1]: Started watchlog.timer - Run watchlog script every 30 second.

root@otus1:~# tail -n 1000 /var/log/syslog  | grep Master
2025-04-04T15:05:59.587003+00:00 otus1 root: Fri Apr  4 03:05:59 PM UTC 2025: I found the word 'ALERT', Master!
2025-04-04T15:06:37.280688+00:00 otus1 root: Fri Apr  4 03:06:37 PM UTC 2025: I found the word 'ALERT', Master!
2025-04-04T15:07:08.338726+00:00 otus1 root: Fri Apr  4 03:07:08 PM UTC 2025: I found the word 'ALERT', Master!


root@otus1:~# apt install spawn-fcgi php php-cgi php-cli \
 apache2 libapache2-mod-fcgid -y


root@otus1:~# mkdir -p /etc/spawn-fcgi

root@otus1:~# touch /etc/spawn-fcgi/fcgi.conf /etc/systemd/system/spawn-fcgi.service

/etc/spawn-fcgi/fcgi.conf:
# You must set some working options before the "spawn-fcgi" service will work.
# If SOCKET points to a file, then this file is cleaned up by the init script.
#
# See spawn-fcgi(1) for all possible options.
#
# Example :
SOCKET=/var/run/php-fcgi.sock
OPTIONS="-u www-data -g www-data -s $SOCKET -S -M 0600 -C 32 -F 1 -- /usr/bin/php-cgi"



/etc/systemd/system/spawn-fcgi.service:
[Unit]
Description=Spawn-fcgi startup service by Otus
After=network.target

[Service]
Type=simple
PIDFile=/var/run/spawn-fcgi.pid
EnvironmentFile=/etc/spawn-fcgi/fcgi.conf
ExecStart=/usr/bin/spawn-fcgi -n $OPTIONS
KillMode=process

[Install]
WantedBy=multi-user.target



root@otus1:~# systemctl start spawn-fcgi

root@otus1:~# systemctl status spawn-fcgi
● spawn-fcgi.service - Spawn-fcgi startup service by Otus
     Loaded: loaded (/etc/systemd/system/spawn-fcgi.service; disabled; preset: enabled)
     Active: active (running) since Sun 2025-04-06 18:59:22 UTC; 9s ago
   Main PID: 11253 (php-cgi)
      Tasks: 33 (limit: 2272)
     Memory: 14.7M (peak: 15.1M)
        CPU: 245ms
     CGroup: /system.slice/spawn-fcgi.service
             ├─11253 /usr/bin/php-cgi
             ├─11258 /usr/bin/php-cgi
             ├─11259 /usr/bin/php-cgi
             ├─11260 /usr/bin/php-cgi
             ├─11261 /usr/bin/php-cgi
             ├─11262 /usr/bin/php-cgi
             ├─11263 /usr/bin/php-cgi
             ├─11264 /usr/bin/php-cgi
             ├─11265 /usr/bin/php-cgi
             ├─11266 /usr/bin/php-cgi
             ├─11267 /usr/bin/php-cgi
             ├─11268 /usr/bin/php-cgi
             ├─11269 /usr/bin/php-cgi
             ├─11270 /usr/bin/php-cgi
             ├─11271 /usr/bin/php-cgi
             ├─11272 /usr/bin/php-cgi
             ├─11273 /usr/bin/php-cgi
             ├─11274 /usr/bin/php-cgi
             ├─11275 /usr/bin/php-cgi
             ├─11276 /usr/bin/php-cgi
             ├─11277 /usr/bin/php-cgi
             ├─11278 /usr/bin/php-cgi
             ├─11279 /usr/bin/php-cgi
             ├─11280 /usr/bin/php-cgi
             ├─11281 /usr/bin/php-cgi
             ├─11282 /usr/bin/php-cgi
             ├─11283 /usr/bin/php-cgi
             ├─11284 /usr/bin/php-cgi
             ├─11285 /usr/bin/php-cgi
             ├─11286 /usr/bin/php-cgi
             ├─11287 /usr/bin/php-cgi
             ├─11288 /usr/bin/php-cgi
             └─11289 /usr/bin/php-cgi

Apr 06 18:59:22 otus1 systemd[1]: Started spawn-fcgi.service - Spawn-fcgi startup service by Otus.

root@otus1:~# apt install nginx -y

/etc/systemd/system/nginx@.service:
# Stop dance for nginx
# =======================
#
# ExecStop sends SIGSTOP (graceful stop) to the nginx process.
# If, after 5s (--retry QUIT/5) nginx is still running, systemd takes control
# and sends SIGTERM (fast shutdown) to the main process.
# After another 5s (TimeoutStopSec=5), and if nginx is alive, systemd sends
# SIGKILL to all the remaining processes in the process group (KillMode=mixed).
#
# nginx signals reference doc:
# http://nginx.org/en/docs/control.html
#
[Unit]
Description=A high performance web server and a reverse proxy server
Documentation=man:nginx(8)
After=network.target nss-lookup.target

[Service]
Type=forking
PIDFile=/run/nginx-%I.pid
ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx-%I.conf -q -g 'daemon on; master_process on;'
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx-%I.conf -g 'daemon on; master_process on;'
ExecReload=/usr/sbin/nginx -c /etc/nginx/nginx-%I.conf -g 'daemon on; master_process on;' -s reload
ExecStop=-/sbin/start-stop-daemon --quiet --stop --retry QUIT/5 --pidfile /run/nginx-%I.pid
TimeoutStopSec=5
KillMode=mixed

[Install]
WantedBy=multi-user.target

root@otus1:~# cp /etc/nginx/nginx.conf /etc/nginx/nginx-first.conf

root@otus1:~# cp /etc/nginx/nginx.conf /etc/nginx/nginx-second.conf

root@otus1:~# systemctl start nginx@first

root@otus1:~# systemctl start nginx@second

root@otus1:~# systemctl status nginx@first
● nginx@first.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/etc/systemd/system/nginx@.service; disabled; preset: enabled)
     Active: active (running) since Sun 2025-04-06 19:07:58 UTC; 3min 5s ago
       Docs: man:nginx(8)
    Process: 11647 ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx-first.conf -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 11648 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx-first.conf -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 11650 (nginx)
      Tasks: 3 (limit: 2272)
     Memory: 2.3M (peak: 2.8M)
        CPU: 68ms
     CGroup: /system.slice/system-nginx.slice/nginx@first.service
             ├─11650 "nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx-first.conf -g daemon on; master_process on;"
             ├─11651 "nginx: worker process"
             └─11652 "nginx: worker process"

Apr 06 19:07:58 otus1 systemd[1]: Starting nginx@first.service - A high performance web server and a reverse proxy server...
Apr 06 19:07:58 otus1 systemd[1]: Started nginx@first.service - A high performance web server and a reverse proxy server.

root@otus1:~# systemctl status nginx@second
● nginx@second.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/etc/systemd/system/nginx@.service; disabled; preset: enabled)
     Active: active (running) since Sun 2025-04-06 19:10:18 UTC; 50s ago
       Docs: man:nginx(8)
    Process: 11786 ExecStartPre=/usr/sbin/nginx -t -c /etc/nginx/nginx-second.conf -q -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
    Process: 11789 ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx-second.conf -g daemon on; master_process on; (code=exited, status=0/SUCCESS)
   Main PID: 11795 (nginx)
      Tasks: 3 (limit: 2272)
     Memory: 2.4M (peak: 2.5M)
        CPU: 66ms
     CGroup: /system.slice/system-nginx.slice/nginx@second.service
             ├─11795 "nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx-second.conf -g daemon on; master_process on;"
             ├─11796 "nginx: worker process"
             └─11797 "nginx: worker process"

Apr 06 19:10:18 otus1 systemd[1]: Starting nginx@second.service - A high performance web server and a reverse proxy server...
Apr 06 19:10:18 otus1 systemd[1]: Started nginx@second.service - A high performance web server and a reverse proxy server.


root@otus1:~# ss -tnulp | grep nginx
tcp   LISTEN 0      511                   0.0.0.0:9002      0.0.0.0:*    users:(("nginx",pid=11797,fd=5),("nginx",pid=11796,fd=5),("nginx",pid=11795,fd=5))                 
tcp   LISTEN 0      511                   0.0.0.0:9001      0.0.0.0:*    users:(("nginx",pid=11652,fd=5),("nginx",pid=11651,fd=5),("nginx",pid=11650,fd=5))                 

root@otus1:~# ps afx | grep nginx
  11823 pts/6    S+     0:00                                      \_ grep --color=auto nginx
  11650 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx-first.conf -g daemon on; master_process on;
  11651 ?        S      0:00  \_ nginx: worker process
  11652 ?        S      0:00  \_ nginx: worker process
  11795 ?        Ss     0:00 nginx: master process /usr/sbin/nginx -c /etc/nginx/nginx-second.conf -g daemon on; master_process on;
  11796 ?        S      0:00  \_ nginx: worker process
  11797 ?        S      0:00  \_ nginx: worker process
```
