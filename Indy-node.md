# Indy node

Install Indy Node
```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys CE7709D068DB5E88
sudo bash -c 'echo "deb https://repo.sovrin.org/deb xenial stable" >> /etc/apt/sources.list'
sudo apt-get update
sudo apt-get install indy-node
```
Go to `/etc/indy/indy_config.py` and change `NETWORK_NAME=''` as you want.

Generate indy pool
```sh
generate_indy_pool_transactions --nodes 4 --clients 5 --nodeNum 1 2 3 4
```

Start Nodes
```sh
start_indy_node Node1 0.0.0.0 9701 0.0.0.0 9702
start_indy_node Node2 0.0.0.0 9703 0.0.0.0 9704
start_indy_node Node3 0.0.0.0 9705 0.0.0.0 9706
start_indy_node Node4 0.0.0.0 9707 0.0.0.0 9708
```

## For Supervisor
```
[program:node1]
command=start_indy_node Node1 0.0.0.0 9701 0.0.0.0 9702
autostart=true
autorestart=true
directory=/home/indy
stdout_logfile=/var/log/indy/node1.log
stderr_logfile=/var/log/indy/node1.log

[program:node2]
command=start_indy_node Node2 0.0.0.0 9703 0.0.0.0 9704
autostart=true
autorestart=true
directory=/home/indy
stdout_logfile=/var/log/indy/node2.log
stderr_logfile=/var/log/indy/node2.log

[program:node3]
command=start_indy_node Node3 0.0.0.0 9705 0.0.0.0 9706
autostart=true
autorestart=true
directory=/home/indy
stdout_logfile=/var/log/indy/node3.log
stderr_logfile=/var/log/indy/node3.log

[program:node4]
command=start_indy_node Node4 0.0.0.0 9707 0.0.0.0 9708
autostart=true
autorestart=true
directory=/home/indy
stdout_logfile=/var/log/indy/node4.log
stderr_logfile=/var/log/indy/node4.log

priority=1000
```
Add above to `/etc/supervisor/conf.d/indy.conf`.

Run below codes
```bash
sudo supervisorctl reread
sudo supervisorctl update
```
