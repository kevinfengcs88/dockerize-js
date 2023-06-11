# dockerize-js

This is a project which Dockerizes a basic NodeJS app. The app simply uses express to listen on port 3000 and logs a message to the screen.

## How to Dockerize locally

*I'm assuming that you have Docker installed on your machine already*
1. Clone this repository to your machine
2. Run `docker build -t {name:tag}`. For example, `docker build -t node-app:1.0`
3. Run `docker images` and you should see the newly created Docker image (Note: Running `docker ps -a` won't display this, since the image has never been run before&mdash;therefore isn't a container of it).
4. Run `docker run --name {name} -d -p {HOST_PORT:CONTAINER_PORT} {name:tag}`. For example, `docker run --name express-app -d -p 6969:3000 node-app:1.0` (Note: It is customary to use the same port for both the host port and the container port, but to illustrate the difference, you should navigate to localhost:6969 in your browser to see the app in this example).
5. Navigate to http://localhost:{HOST_PORT}/ and you should see the app running

##### Common npm error

You may run into error logs which look something like this while building the Docker image:

```
=> ERROR [5/5] RUN npm 86.5s
------
[5/5] RUN npm install:
#0 86.40 npm ERR! code EAI_AGAIN
#0 86.40 npm ERR! syscall getaddrinfo
#0 86.40 npm ERR! errno EAI_AGAIN
#0 86.41 npm ERR! request to https://registry.npmjs.org/express failed, reason: getaddrinfo EAI_AGAIN registry.npmjs.org
#0 86.41 
#0 86.41 npm ERR! A complete log of this run can be found in: /root/.npm/_logs/2023-06-11T21_59_54_622Z-debug-0.log
```

In this case, navigate to `/etc/docker` and create a file called `daemon.json` (use `sudo` if editing with Vim) with these contents:

```
{
  "dns": ["10.0.0.2", "8.8.8.8"]
}
```
