# docker_assignment
This is a repository that builds onto itself based on CP assignments
The current purpose of this repository is to:
- create a docker volume and store webfiles from github repo
- serve the 2048 game through a nginx webserver on Docker through port 8000 using Docker Volume
- serve the dojo-jump game on a second nginx webserver through port 8080 using a bind-mount to access webfiles

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

***Clear the Environment***
