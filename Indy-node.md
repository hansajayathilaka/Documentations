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

Install Libsodium
```bash
wget –N —no–check–certificate https://github.com/jedisct1/libsodium/releases/download/1.0.11/libsodium-1.0.11.tar.gz
tar zfvx libsodium-1.0.11.tar.gz
cd libsodium-1.0.11/
./configure
make && make install

echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf
ldconfig
```

Install Python 3.8
```bash
sudo apt update && sudo apt upgrade
sudo apt install wget build-essential libreadline-gplv2-dev libncursesw5-dev \
     libssl-dev libsqlite3-dev tk-dev libgdbm-dev libc6-dev libbz2-dev libffi-dev zlib1g-dev

wget https://www.python.org/ftp/python/3.8.10/Python-3.8.10.tgz
tar xzf Python-3.8.10.tgz
cd Python-3.8.10
./configure --enable-optimizations
make altinstall
python3.8 -V
```

Change default python version
- Run `vim ~/.bash_profile` and add `alias python='/usr/local/bin/python3.8'` to end of the file.
- Run `source ~/.bash_profile` to apply changes.
- If there is no `.bash_profile` try `.bashrc`.

