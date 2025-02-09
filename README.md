# Docker

## Basic Docker commands :

1. **For pulling images from docker hub :**

```zsh
docker pull <image_name>
```

2. **To create and run a container from an existing image :**

```bash
docker run -it <image_name>
```

Note : `-it` stands for "Interactive".

3. **To list all the images :**

```bash
docker images
```

4. **To list all the active containers :**

```bash
docker ps
```

5. **To list all the containers :**

```bash
docker ps -a
```

6. **To stop a specific container :**

```bash
docker stop <first_3_letters_of_the_container_id or container_name>
```

7. **To remove all the inactive(stopped) container :**

```bash
docker container prune
```

8. **To remove a specific container :**

```bash
docker rm <container_name or container_id > --force
```

Note : use `--force` only when you're trying to remove a running container.

## Dockerizing a simple JavaScript application (simple-app)

**To build the image :**

```bash
docker build -t simple-app .
```

**To create and run a container from the custom image(created by us) :**

```bash
docker run simple-app
```

**To open our application in shell mode :**

```bash
docker run -it simple-app sh
```

then run the following commands

```cmd
ls
node hello.js
```

## Dockerizing a vite React + TypeScript application (react-app)

**To build the image :**

```bash
docker build -t react-app .
```

**To create and run a container from the custom image(created by us) :**

```bash
docker run react-app
```

Note : The above command won't work

**Port Mapping :** Port mapping is a concept in docker that allows us to map ports between the docker container and the host machine.

**To start or create a continer along the port mapping :**

```bash
 docker run -p 5173:5173 react-app
```

Note : This command will also not work due to vite. In order to make it work, we need to make some changes in package.json

```json
"script": {
    "dev": "vite --host"
}
```

Now run the same command i.e `docker run -p 5173:5173 react-app`.

- This will work, but there's another problem. Whenever we make some changes in our code, the changes won't be there on the app that is running on the port 5173. 

- This happens because when we build the docker image and run the container, the code is then copied into that container.  

- In order to overcome this issue we need to further adjust our command.

**Final command to dockerize our react-app**

 ```bash
docker run -p 5173:5173 -v "$(pwd):/app" -v /app/node_modules react-app
 ```

 NOTE : 

 - `"$(pwd):/app"` : This tells the docker to mount the current working directory(local code space ) into the app directory inside the container. 
 
 -  This means that our local code is linked to the container and any changes made locally will immediately be reflected inside the running container.

 - `-v` stands for Volumes, which keeps track of those changes.
 
 - We're using another volume mount to track the dependency changes, so that we don't have to re-build the image when we install new packages.

 ## Publishing the docker image to docker hub(react-app) :

**Step 1** : Login to docker hub through CLI, using the the following command :

```bash
docker login
```

**Step 2** : Then `cd` into the react-app folder and run the following command :

```bash
docker tag react-app amithrao/react-app

# and 

docker push amithrao/react-app
```

## Docker compose (react-app-compose) : 

- It's a tool that allows us to define and manage multi-container docker applications.

- It uses `.yml` file to configure the services, Networks and Volumes for your applications, which enables us to run and scale the entire application with a single command.

- We don't have to run 10 commands seperately to run 10 different containers for one application, all thanks to docker compose. 

- We can list all the information needed to run these 10 containers in a single file and run only one command that automatically triggers running the 10 containers.

### Steps to implement docker compose : 

**Step 1 :** Naviate to the root folder of the project.

**Step 2 :** Run the following command

```bash
docker init
```

NOTE : Using `docker init` we initialize our app with all the files needed.

**Step 3 :** Following questions will be asked

```
? What application platform does your project use? Node
? What version of Node do you want to use? 23.6.0
? Which package manager do you want to use? npm
? Do you want to run "npm run build" before starting your server? No
? What command do you want to use to start the app? npm run dev
? What port does your server listen on? 5173
```