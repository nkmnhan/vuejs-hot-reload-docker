# Vuejs Hot-Reloading in Docker
This is a quick example on how to get hot module reloading working in [vuejs/vue-cli](https://github.com/vuejs/vue-cli) in a local dockerized development setup.

## Set up
In this guide, I used `yarn` to install packages:
```console
$ yarn install
```

## Docker Setup 
Let’s start by creating a `Dockerfile` in the root folder of our project:
``` dockerfile
FROM node:lts-alpine

WORKDIR /app

EXPOSE 8080
CMD [ "yarn", "serve" ]
```
Then create a `docker-compose.yaml` in the root folder of our project:
```yaml
version: '3'
services:
  my-app:
    build:
      context: .
    ports:
    - '8080:8080'
    volumes:
    - .:/app
    environment:
    # Chokidar, which Webpack uses for watching changes has an environment to force the use of polling: CHOKIDAR_USEPOLLING
    - CHOKIDAR_USEPOLLING=true
```

Finally, let’s run our Vue.js app in a Docker container:
```console
$ docker-compose.exe -f docker-compose.yaml build
$ docker-compose.exe -f docker-compose.yaml up
```

## Run the applications
Go to http://localhost:8080:
![Before change](/documents/before-change.png)

And let make some changes in your app:
![Make some changes](/documents/make-some-changes.png)

Then wait for the result:
![After change](/documents/after-change.png)

## Summary
With this setup, all you need to write code in your app then get immediately hot-reloading in the container.

## Reference Documents
https://docs.docker.com/compose/

https://vuejs.org/v2/cookbook/dockerize-vuejs-app.html

https://daten-und-bass.io/blog/enabling-hot-reloading-with-vuejs-and-vue-cli-in-docker

https://stackoverflow.com/questions/44643045/running-development-server-with-create-react-app-inside-of-a-docker-container