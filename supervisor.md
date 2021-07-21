## Inastall Supervisor
```
sudo apt-get install supervisor
sudo service supervisor restart
```

## Add service to Supervisor

__Add this config file to the /etc/supervisor/conf.d/<serviceName>.conf__
```
[program:<ProgreamDisplayName>]
; Command for run the service
command=/home/mysite/bin/celery worker -A mysite --loglevel=INFO
; Path to the project
directory=/home/mysite/mysite
; 'www-data' user for external Access
user=<userName>
group=<userGroupName>
numprocs=1
; Log file Path
stdout_logfile=/home/mysite/logs/celery.log
; Error Log path
stderr_logfile=/home/mysite/logs/celery.log
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

