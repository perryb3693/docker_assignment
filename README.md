# docker_assignment
This is a repository that builds onto itself based on CP assignments
The current purpose of this repository is to:
- create a docker volume and store webfiles from github repo
- serve the 2048 game through a nginx webserver on Docker through port 8000 using Docker Volume
- serve the dojo-jump game on a second nginx webserver through port 8080 using a bind-mount to access webfiles
- use docker networks to assign containers an IP address and bridge networks to ensure that they  can communicate with eachother in local network
#- use docker networks to allow containers to use the host machine's network stack through the host network
- set up a Docker container running a Postgres database and connect it to the "to-do-list" Python Flask application
	-(optional) show the configuration for creating a postres database using a volume
- 


***Create a Docker Volume***
Create a new docker volume and copy webfiles over from the gitgub repository
```
docker volume create docker-2048-vol
docker volume ls 				#verify volume creation
```
Make a new directory on the local system that will be referenced later by Docker to pull relevant web files 
```
mkdir /var/lib/docker/volumes/docker-2048-vol/nginx-html
cd /var/lib/docker/volumes/docker-2048-vol/nginx-html
git clone https://github.com/chandradeoarya/2048
cd 2048
mv * .. #move all files to the parent directory
```

***2048 NGINX Website using Docker Volume***
We will use NGINX as the webserver to host the 2048 webgame. Ensure docker is installed on your system and deploy a NGINX container running in the background (detached -d) and map TCP port 80 in the NGINX container to port 8000 on the docker host (-p)
```
docker run -d -p 8000:80 -v /var/lib/docker/volumes/docker-2048-vol/nginx-html:/usr/share/nginx/html --name 2048 nginx
```
Verify container creation
```
docker ps -a
```
Navigate to the website using a web browser on the localhost system
```
http:localhost:8000
```

***Create a new directory on localhost system***
Make a new directory on the local system that will be referenced later by Docker to pull relevant web files 
```
mkdir ~/nginx-html
```
Copy over webfiles from github to localhost directory
```
cd ~/nginx-html
git clone https://github.com/chandradeoarya/dojo-jump
cd dojo-jump
mv * ..
```

***Dojo-Jump NGINX Website using Bind-Mount***
We will use NGINX as the webserver to host the 2048 webgame. Ensure docker is installed on your system and deploy a NGINX container running in the background (detached -d) and map TCP port 80 in the NGINX container to port 8080 on the docker host (-p)
```
docker run -d -p 8080:80 -v ~/nginx-html:/usr/share/nginx/html --name dojo-jump nginx
```
Verify container creation and navigate to site using a browser on the localhost system
```
docker ps -a
```
```
http:localhost:8080
```

***Container Communication***
Create container using a Ubuntu image to test connectivity between containers using ping
```
docker run -ti --name test_cont ubuntu                          #run a Ubuntu container named "test_cont" in a terminal interactive mode (-ti)
```
***Bridge Network***
Use a newly configured bridge network in Docker to allow containers on the same host to communicate with eachother using the network bridge.

Disconnect containers from the default network bridge
```
docker network disconnect bridge test_cont
docker network disconnect bridge 2048_1
docker network disconnect bridge dojo-jump
```
Conduct a ping test through the test_cont container terminal to show no internal or external connectivity. Configure a new network bridge:
```
docker network ls 						#list the available docker networks
docker network inspect bridge | less				#show details of the bridge network and what containers are currently assigned to it
docker network create --driver bridge docker_bridge_net 	#create a bridge network named "docker_bridge_net"
docker network ls						#verify creation of the bridge network
```
Add containers to the new network bridge and use the ping command to verify internal connectivity between containers. View web application to verify connectivity to localhost
```
docker network connect docker_bridge_net test_cont
docker network connect docker_bridge_net 2048_1 
docker network connect docker_bridge_net dojo-jump
```

***Flask App using Postgres Database Container on Host Network***
Set up a new Docker Container running a Postgres database and connect it to the "to-do-list" Python Flask application. The Flask application will use SQLAlchemy to interact with the database. Run the Postgres container using the host network instead of using a bridge network and port mapping. Use host networking in Docker to allow containers to directly use the localsystem's network stack. This is useful when containers need access to services running on the host machine or if low-level access is required to network resources

First, create the Postgres database and configure it to run on the host network
```
docker run -d --network host --name mysql-container -e MYSQL_ROOT_PASSWORD=secret-pw -d mysql:latest
```








docker run -d --name web-server --network host nginx 		#starts a new nginx container named "web-server" and enables host networking "--network host"


 

***Clear the Environment***
