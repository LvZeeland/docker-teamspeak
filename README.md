mbentley/teamspeak
==================

docker image for TeamSpeak 3 Server
based off of debian:jessie

To pull this image:
`docker pull mbentley/teamspeak`

Note: This Dockerfile will always install the very latest version of TS3 available.

Example usage (no persistent storage; for testing only - you will lose your data when the container is removed):

`docker run -d --name teamspeak -e TS3SERVER_LICENSE=accept -p 9987:9987/udp -p 30033:30033 -p 10011:10011 -p 41144:41144 mbentley/teamspeak`

### License Agreement

Starting with [TeamSpeak 3.1.0](https://support.teamspeakusa.com/index.php?/Knowledgebase/Article/View/344/16/how-to-accept-the-server-license-agreement-server--310), the license agreement must be accepted before you can run TeamSpeak.  To accept the agreement, you need to do one of the following:
  * Add the following to your run command: `-e TS3SERVER_LICENSE=accept` to pass an environment variable
  * Add a command line parameter at the end of your run command to accept the license `license_accepted=1`
  * Create a file named `.ts3server_license_accepted` in the persistent storage directory

### Advanced usage with persistent storage:

1. On your host, create necessary directories, files, and set permissions:
  * `mkdir -p /data/teamspeak`
  * `chown -R 503:503 /data/teamspeak`

2. Start container:
    ```
    docker run -d --restart=always --name teamspeak \
      -e TS3SERVER_LICENSE=accept \
      -p 9987:9987/udp -p 30033:30033 -p 10011:10011 -p 41144:41144 \
      -v /data/teamspeak:/data \
      mbentley/teamspeak
    ```

In order to get the credentials for your TS server, check the container logs as it will output the `serveradmin` password and your `ServerAdmin` privilege key.

For additional parameters, check the `(6) Commandline Parameters` section of the [TeamSpeak 3 Server Quickstart Guide](http://media.teamspeak.com/ts3_literature/TeamSpeak%203%20Server%20Quick%20Start.txt).  Either add the parameters to `ts3server.ini` or specify them after the Docker image name.

Example:
```
docker run -d --restart=always --name teamspeak \
  -e TS3SERVER_LICENSE=accept \
  -p 9987:9987/udp -p 30033:30033 -p 10011:10011 -p 41144:41144 \
  -v /data/teamspeak:/data \
  mbentley/teamspeak \
  clear_database=1 \
  create_default_virtualserver=0
```
