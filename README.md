# docker_assignment
This is a repository that builds onto itself based on CP assginements
The current purpose of this repository is to:
- serve the 2048 game through a nginx webserver on Docker through port 8000

***Create a new directory and copy over required webfiles***
Make a new directory on the local system that will be referenced later by Docker to pull relevant web files 
'''
mkdir ~/nginx-html
cd ~/nginx-html
git clone https://github.com/chandradeoarya/2048
'''

***Create NGINX Docker Container***
We will use NGINX as the webserver to host the 2048 webgame. Ensure docker is installed on your system and deploy a NGINX container running in the background (detached -d) and map TCP port 80 in the NGINX container to port 8000 on the docker host (-p)
'''
docker run -d -p 8000:80 -v ~/nginx-html:/usr/share/nginx/html --name testsite nginx
'''
Verify container creation and navigate to site using a browser on the localhost system
'''
docker ps -a
'''
'''
http:localhost:8000
'''

