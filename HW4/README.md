PART1:
docker build -t "hw4" .
sudo docker run -td --name server hw4
sudo docker exec -td server sh -c "echo 'Hello World'>/Hello.txt"

sudo docker exec -it server bash
socat tcp-l:9001,reuseaddr,fork system:'cat /Hello.txt',nofork

sudo docker run -td --name client -h client --link server:server hw4
sudo docker exec -it client bash
curl server:9001


PART2 :

Install docker-compose on both DO Droplets
	docker compose up

Confirm that the redis server is up :
    docker run -t -i --rm --link redis:redis relateiq/redis-cli
    redis 172.17.0.136:6379> ping
    PONG
   
Use docker compose to bring up the Redis Ambassador container on Client-Server
    docker compose up

Run the redis CLI as client this same droplet
    docker run -i -t --rm --link redis-ambassador:redis relateiq/redis-cli
    ping
    set 
    get


PART 3: 

post-receive file in the green.git/hooks and blue.git/hooks, does the following tasks when App pushes to the remote 'green/blue' repo:
    * Builds a new docker image : 'greenimage'
    * Tags the image as 'localhost:5000/greennew:latest' and pushes to local registry
    * Pulls the image from the local repository
    * Stop and remove any existing container with the name 'green'
    * Instantiate a docker container 'green' , map its port 8080 to port 300X on host machine, and deploy the app

We can then view the image added to the docker images with 'sudo docker images'
We can view the app deployed on the container by browsing to the 'localhost:300X'
