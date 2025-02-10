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

## Dockerizing a simple JavaScript application (01_simple-app)

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

## Dockerizing a vite React + TypeScript application (02_react-app)

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

 ## Publishing the docker image to docker hub(02_react-app) :

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

## Docker compose (03_react-app-compose) : 

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

## Docker compose watch(04_mern-docker) :

- Only `docker compose` alone, isn't optimal for developer experience.

- Everytime there is a change in the packages file, we have to re-run the container.

- Yes, docker-compose solves the problem of showing upto date code changes through volumes, letting us manage multiple contaners in a single file and lets us do both things, building and running images.

- But it still doesn't do it automatically, when we change something related to package file or when we think it's needed to rebuild the image.

- This is where the `watch` feature comes in. 

- Docker compose watch listens to our changes and does something like

    - Rebuilding our app
    - Re-running the container

### Things that Docker Compose watch can do 

1. **Sync :** This operation moves the changed files from our computer to the right places in the contianer, making sure that everything stays upto date in real time.

2. **Rebuild :** The rebuild process starts with the creation of new container images and then it updates the services. This is benificial when rolling out changes to applications in production, guaranteeing the most recent version of the code is in operation.

3. **Sync-Restart :** This operation merges the Sync and Rebuild processes.

### Steps for containerizing the full-stack mern application :

**Step 1 :** Navigate to the frontend folder and create `Dockerfile` and `.dockerignore` file.

**Step 2 :** Write all the steps inside `Dockerfile` to create the image and run the container. Add the `node_modules` folder to the `.dockerignore`.   

**Step 3 :** Navigate to the backend folder and create `Dockerfile` and `.dockerignore` file.

**Step 4 :** Write all the steps inside `Dockerfile` to create the image and run the container. Add the `node_modules` folder to the `.dockerignore`.

**Step 5 :** Create a `compose.yaml` file, which links all the containers and contians the instruction to run those contianers.

**Step 6 :** After creating the compose.yml file, run the following command
```bash
docker compose up
```

This command alone isn't enough, because the changes made in the code on your laptop will not be reflected on the browsers localhost. To overcome this, run the following command in new terminal

```bash
docker compose watch
```

## Docker Scout : 

- Container images consist of layers and software packages, which are susceptible to vulnerabilities. These vulnerabilities can compromise the security of containers and applications.

- Docker Scout is a solution for proactively enhancing your software supply chain security. 

- By analyzing your images, Docker Scout compiles an inventory of components, also known as a Software Bill of Materials (SBOM). 

- After careful analysis, docker scout creates a SBOM.

- The SBOM is matched against a continuously updated vulnerability database to pinpoint security weaknesses.

- Docker Scout is a standalone service and platform that you can interact with using Docker Desktop, Docker Hub, the Docker CLI, and the Docker Scout Dashboard. 

- Docker Scout also facilitates integrations with third-party systems, such as container registries and CI platforms

## Dockerizing a Full-stack NextJs application (05_next-docker) :

**Step 1 :** Run the following command.

```bash
docker init
```

**Step 2 :** Re-write all the commands in Dockerfile and compose.yaml file. 

**Step 3 :** Then run the fillowing command.

```bash
docker compose up
```

```bash
docker compose watch
```