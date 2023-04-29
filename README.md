# MC-Compose-Stack
This Docker Compose Stack can be used to host a Proxy Server with multiple sub server, if you have a bit of knowledge about Docker and Compose files you can also instead of a proxy with multiple sub servers just run multiple Minecraft server all controlled by this one compose stack. I will only give limited support for the compose stack but the usage is very simple:

## Usage:
1. Download the Repository with `git clone git@github.com:TimyStream/mc-compose-stack.git` (For SSH) or with `git clone https://github.com/TimyStream/mc-compose-stack.git` (For HTTPS) or just click on the green "Code" button and click on "Download ZIP"
    1. If you downloaded the ZIP file please unpack to a folder of your choice
2. now you just need to simply create a folder called `proxy` and `lobby` on Linux & MacOS for example with `mkdir proxy lobby`.
3. after you have created your folders you need to download the Minecraft server software for the proxy and the lobby.
    - My personal preference for a proxy is the `Velocity` proxy from `PaperMC`
        - You can download the Velocity proxy from [https://papermc.io/software/velocity](https://papermc.io/software/velocity) . This can be done with `curl` or `wget` or your program of choice.
            - the downloaded file just needs to be put in the `proxy` folder for now
        - **DISCLAIMER**: I am **not** working together with [PaperMC](https://papermc.io/) in any kind! It's just my personal preference!
    - For the Lobby Server i suggest to use `PaperMC` which can be downloaded from [https://papermc.io/software/paper](https://papermc.io/software/paper) 
        - This can be done with `curl` or `wget` or your program of choice.
            - the downloaded file just needs to be put in the `lobby` folder for now
        - **DISCLAIMER**: I am **not** working together with [PaperMC](https://papermc.io/) in any kind! It's just my personal preference!
4. Now you need to edit the compose stack file directly:
```YAML
environment:
      - SERVERJAR=[ServerSoftwareFile] # <== there you place the name of the Jar file from the software with ".jar"!
      - ARGS="" # <== In the parentheses you put all the java arguments you have for your server!
volumes:
      - ./[Your Server folder name]:/app # <== also have a look that this is right, your local folder should be mounted to "/app" in the container!
user: 1000:1000 # <== This need to be the exact same IDs with your user who created the folders and starts the Docker Compose stack you can find your id by running "id" in your terminal there should be a "UID and a GID" the first on in the compose file is the "UID" and the second one is the "GID" do not delete the colon between these two numbers and also do not put a space between the IDs and the colon
```
- If you have problems or don't know where to put what, please have a look in the [example compose stack](./examples/docker-compose.yml) file, this is the one i used to test all this.
5. Configure your servers to your liking! The proxy will after the creation not be directly connected with the lobby server. This is something you have to do manually but you can address every server with its "service" name e.g.: `proxy` or `lobby`. For this have a look at the [velocity.toml](./examples/velocity-proxy/velocity.toml) file in the examples folder on line **75**.

## Add new Server:
Go ahead and create a new Folder for the Server and Download your desired Server software as before. To round it up you need to add the section below to the `docker-compose.yml` file. That this snippet works you have to change all the data where the `[ ]` (parenthesis) are right now. In the parenthesis is standing what is supposed to go there.   

```YAML
[your servername]:
    image: ghcr.io/timystream/mjrt:17
    environment:
      - SERVERJAR=[ServerSoftwareFile]
    expose:
      - 25565
    volumes:
      - ./[Your Server folder name]:/app
    stdin_open: true
    tty: true
    user: [your user ID]:[your group id]
    depends_on:
      - proxy
```