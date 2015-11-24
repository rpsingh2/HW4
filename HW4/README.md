## Homework 4 - Advanced Docker
In this homework assignment, you'll get to practice several common architectural patterns for dealing with multiple docker containers.

#### File I/O
###### You want to create a container for a legacy application. You succeed, but you need access to a file that the legacy app creates

1. To Build the image


    * docker build -t "hw4"
    
2. Mapping file access to read file container using SOCAT


    * sudo docker run -td --name server hw4
    * sudo docker exec -td server sh -c "echo 'Hello World'>/Hello.txt"
    * sudo docker exec -it server bash
    * socat tcp-l:9001,reuseaddr,fork system:'cat /Hello.txt',nofork

3. Linked container to the server container


    * sudo docker run -td --name client -h client --link server:server hw4

4. To get inside the client container


    * sudo docker exec -it client bash
    
5. Get the content of the file out.txt located in server containerr


    * curl server:900    

#### Ambassador Pattern
###### Implement the remote ambassador pattern to encapsulate access to a redis container by a container on a different host.

1. Install docker-compose on both DO Droplets


     * docker compose up
    
2. Confirm that the redis server is up 


     * docker run -t -i --rm --link redis:redis relateiq/redis-cli
     * redis 172.17.0.136:6379> ping
     * PONG
   


3. Run the redis CLI as client this same droplet


    * docker run -i -t --rm --link redis-ambassador:redis relateiq/redis-cli
    * ping
    * set 
    * get


#### Docker Deploy
###### Extend the deployment workshop to run a docker deployment process.

post-receive file in the green.git/hooks and blue.git/hooks, does the following tasks when App pushes to the remote 'green/blue' repo:

    * Builds a new docker image : 'greenimage'
    * Tags the image as 'localhost:5000/greennew:latest' and pushes to local registry
    * Pulls the image from the local repository
    * Stop and remove any existing container with the name 'green'
    * Instantiate a docker container 'green' , map its port 8080 to port 300X on host machine, and deploy the app

We can then view the image added to the docker images with 'sudo docker images'
We can view the app deployed on the container by browsing to the 'localhost:300X'


### NOTE :
	
	All gifs have been included in respective folders.	

