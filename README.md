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

4. **To list all the containers :**

```bash
docker ps
```

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