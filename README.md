[![Docker Pulls](https://img.shields.io/docker/pulls/mashape/kong.svg)](https://hub.docker.com/r/elleflorio/svn-server/)

# Description
Lightweight container providing an SVN server, based on **Alpine Linux** and S6 process management (see [here](https://github.com/smebberson/docker-alpine) for details).
The access to the server is possible via **WebDav protocol** (http://), and via **custom protocol** (svn://).

# Running Commands
To run the image, you can use the following command:
```
docker run -d --name svn-server -p 80:80 -p 3960:3960 elleflorio/svn-server
```
You can optionally bind a local folder to the container folder that will store your repositories using the flag `-v <hostpath>:/home/svn`.

# Configuration
**You need to setup username and password** for the access via WebDav protocol. You can use the following command from you host machine:
```
docker exec -t svn-server htpasswd -b /etc/subversion/passwd <username> <password>
```
To verify that everything is up and running, open you browser and connect to `http://localhost/svn`. The system should ask you for the username and password, then it will show you and empty folder (no repos yet!).
Check also that the custom protocol is working fine: go to you terminal and type `svn info svn://localhost:3960`. The system should connect to the server and tell you that is not able to find any repository.
For further information on how to configure Subversion, please refer to the [official web page](https://subversion.apache.org/).