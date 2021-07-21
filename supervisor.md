## Inastall Supervisor
```
sudo apt-get install supervisor
sudo service supervisor restart
```

## Add service to Supervisor

__Add this config file to the `/etc/supervisor/conf.d/<serviceName>.conf`__
```
[program:<ProgreamDisplayName>]
; Command for run the service
command=/home/mysite/bin/celery -A mysite worker --loglevel=INFO
; Path to the project
directory=/home/mysite/mysite
; 'www-data' user for external Access
user=<userName>
group=<userGroupName>
numprocs=1
; Log file Path
stdout_logfile=/home/mysite/logs/celery.log
stdout_logfile_maxbytes = 50MB
stdout_logfile_backups = 10
; Error Log path
stderr_logfile=/home/mysite/logs/celery.log
stderr_logfile_maxbytes = 50MB
stderr_logfile_backups = 10
autostart=true
autorestart=true
startsecs=10
; Need to wait for currently executing tasks to finish at shutdown.
; Increase this if you have very long running tasks.
stopwaitsecs = 600
stopasgroup=true
; Set Celery priority higher than default (999)
; so, if rabbitmq is supervised, it will start first.
priority=1000
```
	
## update config and update the supervisor
	
```
sudo supervisorctl reread
sudo supervisorctl update
```

## Remove service from Supervisor

goto the config file path
`cd /etc/supervisor/conf.d/`

Remove Config file
`sudo rm <serviceName>.conf`

## update config and update the supervisor
```
sudo supervisorctl reread
sudo supervisorctl update
```

