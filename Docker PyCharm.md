## Setup Docker Remote Interpreter PyCharm
---
1. Create The Dockerfile and docker-compose.yml file
2. Open Settings (Ctrl+Alt+S or click Settings on the main toolbar) and click the Docker page under the Build, Execution, Deployment node. Click Add a docker server to create a Docker server. Add These Settings And Save.
    + Select `TCP Socket`
    + Engine API URL: `unix:///var/run/docker.sock`
    + If Connection Error, Follow These Steps:
        + `sudo groupadd docker`
        + `sudo usermod -aG docker $USER`
        + `sudo chmod a+rwx /var/run/docker.sock`
        + `sudo chmod a+rwx /var/run/docker.pid`

3. In Settings Window, Select `Python Interpreter` Under Your Project Name.
4. Click On The `Settings Icon`. Then Click On `Add`
5. Select `Docker Compose`. Select `django` In Service Field And Save.
6. Double Click On Docker Icon To Connect In PyCharm
7. Tools -> Run 'manage.py' task. Type `migrate`
8. Run -> Edit Configurations. In The Dialog Box, Click `Add Run/Debug Configuration` For A Django Server And Select `Django Server`
9. **Host Field Must Be Set To 0.0.0.0- To Make Sure That We Listen To Requests Coming From Outside The Docker Container.** Run This Configuration.