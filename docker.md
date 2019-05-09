# DOCKER NOTES
## Why Docker?
Docker is generally used to completely deploy and run any application with ease. Where it takes several steps to install a particular program (like redis) on any machine, docker does this with one simple command. Docker helps to build, ship and run application complete with all the dependencies. 

## Namespacing in OS: 
Consider an hypothetical situation where, two programs *Chrome* and *NodeJS* on same Operating System requires *python2* and *python3* respectively. And suppose only *python2* is installed, *Chrome* would run without any issues but *NodeJS* would fail. Namespacing solves this issue by segmenting the resources into segments each with respective softwares installed or certain resource access.

## Control Groups in OS:
Similar to namespacing, but it is used to *limit* Memory, CPU Usage, Hardisk I/O and N/W Bandwith. So a particular process could be allocated a specific amount of resource beyond which it cannot use.  

## Docker Beginner-1 Commands: Creating and Running Pre-Built Docker Images
1. **docker run <image_name>** _**(Creating and Running a Container)**_  
`sudo docker run hello-world`  
`sudo docker busybox echo hello world` - Command echos the hello world and immediately exits.  
`sudo docker run hello-world echo hello world` - This command FAILS because hello world container does not have echo command installed, infact it does not have anything installed it just prints a message.  

2. **docker ps** _**(List Containers)**_  
`sudo docker ps` - Initially it wont show up anything since there is no container running in the background  
`sudo docker run busybox ping google.com` - This will keep pinging google.com thus keep the terminal alive, now running 'sudo docker ps' should return a running container  
`sudo docker ps --all` - Lists all the containers that was ever run  

**DOCKER RUN = DOCKER CREATE + DOCKER START**  

3. **docker create <image_name>** _**(Creates a ready to run container and returns the created container's ID)**_  
`sudo docker create hello-world` - It will return the created container's ID  

4. **docker start <created_container_id>** _**(Starts the provided container_id)**_  
`sudo docker start -a <container_id>` - \-a symbolize to "attach" the container to current terminal thus the o/p is displayed  
`sudo docker start <container_id>` - Starts the container but doesn't attach the o/p to current terminal  

## Docker Hub and Image Cache.  
When any image is 'run', it is searched for in the local "Image Cache". If not found it'll be downloaded (pulled) from the "Docker Hub" to local "Image Cache". The next time same image is referenced, it'll skip the downloading and directly 'run' the downloaded image from "Image Cache".  

## Lifecycle of a container.  
On running `sudo docker ps --all`, all the previous running container will be listed. The listed containers will be mostly in EXITED (0) state. To **restart** the EXITED container you just need to run `sudo docker start -a 'container_id_of_the_exited_container'`. This will restart the container with the command that was previously issued. The command passed during container creation cannot be modified during restarting of that container.

## Docker Beginner-1 Commands (Cont.1):  
5. **docker system prune** _**(Completely removes all stopped containers, all networks not used by at least one container, all dangling images and all build image cache)**_  
`sudo docker system prune` - Since this deletes downloaded image cache aswell the next time you try to run or create a container it'll pull the image from docker hub.  

6. **docker logs** _**(Prints the logs/output generated of the running (up) or exited container)**_  
`sudo docker logs <container_id>`  

7. **docker stop** _**(Issues a h/w signal 'SIGTERM' to process, similar to "ctrl+c". process will have some time to stop on it's own.)**_  
`sudo docker stop <container_id>` - If a process doesn't respond to 'SIGTERM' command it'll be killed automatically in 10 seconds.  

8. **docker kill** _**(Issues a h/w signal 'SIGKILL' to process which instantly kills the process)**_  
`sudo docker kill <container_id>` - It is safer to use stop instead of kill  

9. **docker exec** _**(Execute a command within another container)**_  
`sudo docker run redis` Runs a redis-server within the container.	  
`sudo docker exec -it <container_id> <command>"` Executes the "command" within the container provided. "-it" flag is required to get the control back to current terminal.  
`sudo docker exec -it <running_redis_container_id> redis-cli` It'll run the command "redis-cli" inside "redis" container. (Info: redis-cli is the command to run redis queries directly on 'redis-server').  

10. **Open a Shell** _**(access the terminal of the container)**_  
`sudo docker exec -it <running_container_id> sh` 'sh' is nothing but shell program (command processor), instead 'bash' or 'zsh' could also be used.   
`sudo docker run -it <image_name> sh` Very similar to exec, except this container won't execute the default command it was programmed with (e.g: redis server won't start with this command but you'll be directly redirected to shell).  

## Purpose of '-i' and '-t'.  
Every process has 3 ways to communicate with underlying architecture - STDIN, STDOUT and STDERR  
   * \-i = connects the current terminal to containers STDIN  
   * \-t = emulates the terminal so that rsults would be formatted and displayed as it will be displayed in the original cli  

## Containers are Isolated unless programmed to communicate.  
`sudo docker run -it busybox sh` Running this command on two different terminals create two running containers of "busybox" which is mutually exclusive, resources is by default not shared between two.  

## What is a Dockerfile?  
It contains all the configurations to build an image that defines a container behaviour. *Dockerfile* -> Docker Client -> Docker Server -> *Docker Image*. We pass the "Dockerfile" to "Docker Client", "Docker Client" submits the "Dockerfile" to "Docker Server". "Docker Server" reads the Dockerfile and creates the "Docker Image".

## What does Dockerfile Generally Include?
1. **Base Image** : OS on which any program needs to be installed is mentioned here.  
2. **Commands to Install Additional Programs** : Any additional commands to run once the OS has been installed is mentioned here.  
3. **Command to Run on Container Startup** : Command to run on container startup is mentioned here.  

## Docker Begginer-2 Commands: Creating your own Docker Images  
1. **docker build**  
   * Create a folder with a relevant name (e.g: redis-server).
   * Inside the folder create a file named "Dockerfile", without any extension.
   * Populate docker file with following commands (w/o tab):
	```
	FROM alpine
	RUN apk add --update redis
	CMD \["redis-server"\]
	```
   * Save and Exit.
   * Open terminal and run
	`sudo docker build 'folder_path'`
   * Once the operation finishes it will return a Image ID. This could then be used to create containers. 
	`sudo docker run 'image_id'`

## Dockerfile Explained:
Docker file consists of two parts:  
**COMMAND arguments**  
1. **FROM alpine** : OS on which any program needs to be installed is mentioned here. Alpine linux is used as the base image (OS) to install redis software on.
2. **RUN apk add --update redis** : Any additional commands to run once the OS has been installed is mentioned here. We use "apk", a default package manager provided with Alpine, to install redis.
3. **CMD ["redis-server"]** : Command to run on container startup is mentioned here. "redis-server" command starts up the redis server.
