# docker-intro

Docker is an open platform for developing, shipping, and running applications. With Docker, you can separate your applications from your infrastructure and treat your infrastructure like a managed application. Docker helps you ship code faster, test faster, deploy faster, and shorten the cycle between writing code and running code.

Docker does this by combining kernel containerization features with workflows and tooling that helps you manage and deploy your applications.

Docker containers can be directly used in Kubernetes, which allows them to be run in the Kubernetes Engine with ease. After learning the essentials of Docker, you will have the skillset to start developing Kubernetes and containerized applications.

docker run hello-world
docker images
docker run hello-world
docker ps
docker ps -a

mkdir test && cd test

cat > Dockerfile <<EOF
# Use an official Node runtime as the parent image
FROM node:6

# Set the working directory in the container to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
ADD . /app

# Make the container's port 80 available to the outside world
EXPOSE 80

# Run app.js using node when the container launches
CMD ["node", "app.js"]
EOF


cat > app.js <<EOF
const http = require('http');

const hostname = '0.0.0.0';
const port = 80;

const server = http.createServer((req, res) => {
    res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
        res.end('Hello World\n');
});

server.listen(port, hostname, () => {
    console.log('Server running at http://%s:%s/', hostname, port);
});

process.on('SIGINT', function() {
    console.log('Caught interrupt signal and will exit');
    process.exit();
});
EOF

docker build -t node-app:0.1 .
docker images

docker run -p 4000:80 --name my-app node-app:0.1
curl http://localhost:4000

docker stop my-app && docker rm my-app
docker run -p 4000:80 --name my-app -d node-app:0.1
docker ps

docker logs [container_id]
cd test

const server = http.createServer((req, res) => {
    res.statusCode = 200;
      res.setHeader('Content-Type', 'text/plain');
        res.end('Welcome to Cloud\n');
});

docker build -t node-app:0.2 .
docker run -p 8080:80 --name my-app-2 -d node-app:0.2
docker ps

curl http://localhost:8080
curl http://localhost:4000

docker logs -f [container_id]
docker exec -it [container_id] bash

docker inspect [container_id]
docker inspect --format='{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' [container_id]

# PUBLISH

For gcr:

[hostname]= gcr.io
[project-id]= your project's ID
[image]= your image name
[tag]= any string tag of your choice. If unspecified, it defaults to "latest".

gcloud config list project

[core]
project = [project-id]

Your active configuration is: [default]

docker tag node-app:0.2 gcr.io/[project-id]/node-app:0.2
docker images

docker push gcr.io/[project-id]/node-app:0.2

http://gcr.io/[project-id]/node-app

docker stop $(docker ps -q)
docker rm $(docker ps -aq)

docker rmi node-app:0.2 gcr.io/[project-id]/node-app node-app:0.1
docker rmi node:6
docker rmi $(docker images -aq) # remove remaining images
docker images

docker pull gcr.io/[project-id]/node-app:0.2
docker run -p 4000:80 -d gcr.io/[project-id]/node-app:0.2
curl http://localhost:4000





