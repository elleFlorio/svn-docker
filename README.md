[![Docker Image](https://img.shields.io/badge/docker%20image-available-green.svg)](https://hub.docker.com/r/elleflorio/svn-server/)

# Description
Lightweight container providing an SVN server, based on **Alpine Linux** and S6 process management (see [here](https://github.com/smebberson/docker-alpine) for details).
The access to the server is possible via **WebDav protocol** (http://), and via **custom protocol** (svn://).
A complete tutorial on how to build this image, and how to run the container is available on [Medium](https://medium.com/@elle.florio/the-svn-dockerization-84032e11d88d#.bafh3otmh)

# Running Commands
To run the image, you can use the following command:
```
docker run -d --name svn-server -p 80:80 -p 3690:3690 -v <hostpath>:/data elleflorio/svn-server
```

`/data/repositories` stores your repositories and can use either bind mount or named volume. `/data/subversion` stores subversion configuration and `/data/svnadmin` stores SVNADMIN configuration

# Configuration

* http://localhost/svnadmin/ - **You need to change username and password** from default `admin`/`admin` to your

To verify that everything is up and running, open your browser and connect to `http://localhost/svn`. The system should ask you for the username and password, then it will show you an empty folder (no repos yet!).
Check also that the custom protocol is working fine: go to your terminal and type `svn info svn://localhost:3690`. The system should connect to the server and tell you that is not able to find any repository.
For further information on how to configure Subversion, please refer to the [official web page](https://subversion.apache.org/).

# Alternative configuration via SVNADMIN
the image provides a graphical ui using the [SVNADMIN](https://github.com/mfreiholz/iF.SVNAdmin) interface via `http://localhost/svnadmin`.
You'll be prompted with a setup page, remember to test every step on the page then save the configuration.

# How to contribute
I'm super happy if you want to contribute! I do my best to keep this image updated and solve the issues that may arise, but I'm not much an operations guy, and I have very limited free time. :sweat_smile:

If you find something that can be improved or the solution to some issue, just comment the issue to notify that you will handle it, and then submit a pull request. I will then merge it and publish the updated image in the Docker Hub. :wink:

Thank you! :smile:

## Also you can use a docker-compose.yml for start as service

```
version: '3'

services:
  svn_server:
    image: elleflorio/svn-server
    ports:
      - "8011:80" # web port
      - "3690:3690" # web port
    volumes:
      - "./data:/data:rw"
    restart: always
    networks:
      - example_svn_server_net
networks:
    example_svn_server_net:
    driver: bridge
```
