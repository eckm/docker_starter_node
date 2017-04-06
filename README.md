# docker_starter_node
Starter kit for a node webapp runnning on Docker

## Step 1 - Building the image
Go inside the app/ directory and run the following command

```
$ docker build -t <your username>/my-node-webapp .
```
(Don't forget the "." at the end which is the main argument of the "build" command).
This command will build your Docker image and tag it thanks to the -t flag.

Find your image:
```
$ docker images
```

## Step 2 - Run the image
```
$ docker run -p 25689:8080 -d <your username>/my-node-webapp
```
The 'p flag redirects a public port to a private port inside the container (here the port 25689 redirects to port 8080 exposed inside the container - see the Dockerfile where you can see the instruction "EXPOSE 8080"). You can choose any available port.
The -d flag makes the container run in "detached" mode - it leaves the container running in the background.

Check the output of your app:
```
#Get the container ID
$ docker ps

# Print the app output
$ docker logs <container ID>
```
You should see "Running on http://localhost:8080"

## Final step - Test your application

To test your app, you need to get the port that Docker mapped:
```
$ docker ps | grep my-node-webapp

#Example
CONTAINER ID   IMAGE                COMMAND       CREATED          STATUS          PORTS                     NAMES
e31ac1837bc1   eckm/my_node-webapp  "npm start"   18 seconds ago   Up 17 seconds   0.0.0.0:25689->8080/tcp   digital_dungeon
```
Then call your application using curl:
```
$ curl -i http://localhost:25689

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 14
Date: Wed, 22 Feb 2017 16:43:56 GMT
Connection: keep-alive

Hello Amadeus
```
## Extra step - Test your application from Docker directly

You can go inside your Docker container with the "exec" command
```
$ docker exec -it <container id> /bin/bash
```
Then you can run the curl command directly on the port exposed by your node application.
```
root@e31ac1837bc1:/usr/src/app# curl -i http://localhost:8080

HTTP/1.1 200 OK
X-Powered-By: Express
Content-Type: text/html; charset=utf-8
Content-Length: 14
Date: Wed, 22 Feb 2017 16:47:32 GMT
Connection: keep-alive

Hello Amadeus
```
