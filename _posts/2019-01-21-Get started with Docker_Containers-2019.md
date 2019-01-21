---
layout:     post   				    # 使用的布局（不需要改）
title:      Get started with Docker 				# 标题
subtitle:   Containers #副标题
date:       2019-01-21				# 时间
author:     frankie 						# 作者
header-img: img/post-bg-unix-linux.jpg 	#这篇文章标题背景图片
catalog: true 						# 是否归档
tags:								#标签
    - tech
    - k8s
    - docker
---

# Prerequisites
* Install Docker version 1.13 or higher.
* Read the orientation in Part 1.
* Give your environment a quick test run to make sure you’re all set up:

```
docker run hello-world
```

# Introduction
* Stack
* Services
* Container (you are here)


# Define a container with Dockerfile
<b>Dockerfile</b> defines what goes on in the environment inside your container. Access to resources like networking interfaces and disk drives is virtualized inside this environment, which is isolated from the rest of your system, so you need to <b>map ports to the outside world, and be specific about what files you want to “copy in” to that environment.</b> However, after doing that, you can expect that the build of your app defined in this Dockerfile behaves exactly the same wherever it runs.

# Dockerfile
Create an empty directory. Change directories (<b>cd</b>) into the new directory, create a file called <b>Dockerfile</b>, copy-and-paste the following content into that file, and save it. Take note of the comments that explain each statement in your new Dockerfile.

```
# Use an official Python runtime as a parent image
FROM python:2.7-slim

# Set the working directory to /app
WORKDIR /app

# Copy the current directory contents into the container at /app
COPY . /app

# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt

# Make port 80 available to the world outside this container
EXPOSE 80

# Define environment variable
ENV NAME World

# Run app.py when the container launches
CMD ["python", "app.py"]
```

This <b>Dockerfile</b> refers to a couple of files we haven’t created yet, namely <b>app.py</b> and <b>requirements.txt</b>. Let’s create those next.


# The app itself
Create two more files, <b>requirements.txt</b> and <b>app.py</b>, and put them in the same folder with the <b>Dockerfile</b>. This completes our app, which as you can see is quite simple. When the above <b>Dockerfile</b> is built into an image, <b>app.py</b> and <b>requirements.txt</b> is present because of that <b>Dockerfile’s</b> <b>COPY</b> command, and the output from <b>app.py</b> is accessible over HTTP thanks to the <b>EXPOSE</b> command.

```
requirements.txt

Flask
Redis
```

```
app.py

from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"

    html = "<h3>Hello {name}!</h3>" \
           "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

Now we see that pip install -r requirements.txt installs the Flask and Redis libraries for Python, and the app prints the environment variable NAME, as well as the output of a call to socket.gethostname(). Finally, because Redis isn’t running (as we’ve only installed the Python library, and not Redis itself), we should expect that the attempt to use it here fails and produces the error message.

Note: Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.

That’s it! You don’t need Python or anything in requirements.txt on your system, nor does building or running this image install them on your system. It doesn’t seem like you’ve really set up an environment with Python and Flask, but you have.

# Build the app
We are ready to build the app. Make sure you are still at the top level of your new directory. Here’s what ls should show:
```
$ ls
Dockerfile		app.py			requirements.txt
```

Now run the build command. This creates a Docker image, which we’re going to name using the --tag option. Use -t if you want to use the a shorter option.
```
docker build --tag=friendlyhello .
```

Where is your built image? It’s in your machine’s local Docker image registry:
```
$ docker image ls

REPOSITORY            TAG                 IMAGE ID
friendlyhello         latest              326387cea398
```

Note how the tag defaulted to latest. The full syntax for the tag option would be something like --tag=friendlyhello:v0.0.1.


# Run the app
Run the app, mapping your machine’s port 4000 to the container’s published port 80 using -p:
```
docker run -p 4000:80 friendlyhello
```
You should see a message that Python is serving your app at http://0.0.0.0:80. But that message is coming from inside the container, which doesn’t know you mapped port 80 of that container to 4000, making the correct URL http://localhost:4000.

Go to that URL in a web browser to see the display content served up on a web page.

![Imgur](https://i.imgur.com/ZEznYIm.png)


You can also use the curl command in a shell to view the same content.
```
$ curl http://localhost:4000

<h3>Hello World!</h3><b>Hostname:</b> 8fc990912a14<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
```

This port remapping of 4000:80 demonstrates the difference between EXPOSE within the Dockerfile and what the publish value is set to when running docker run -p. In later steps, map port 4000 on the host to port 80 in the container and use http://localhost.

Hit CTRL+C in your terminal to quit.


Now let’s run the app in the background, in detached mode:
```
docker run -d -p 4000:80 friendlyhello
```

```
$ docker container ls
CONTAINER ID        IMAGE               COMMAND             CREATED
1fa4ab2cf395        friendlyhello       "python app.py"     28 seconds ago
```

```
docker container stop [container_id]
```

# Share your image
## Log in with your Docker ID
```
docker login
```

## Tag the image
```
docker tag image username/repository:tag

e.g.
docker tag friendlyhello gordon/get-started:part2
```

```
$ docker image ls

REPOSITORY               TAG                 IMAGE ID            CREATED             SIZE
friendlyhello            latest              d9e555c53008        3 minutes ago       195MB
gordon/get-started         part2               d9e555c53008        3 minutes ago       195MB
python                   2.7-slim            1c7128a655f6        5 days ago          183MB
...
```

## Publish the image
```
docker push username/repository:tag

docker push eyan422/docker-get-started:part2
```

## Pull and run the image from the remote repository
From now on, you can use docker run and run your app on any machine with this command:
```
docker run -p 4000:80 username/repository:tag

docker run -p 4000:80 eyan422/docker-get-started:part2
```

```
$ docker run -p 4000:80 gordon/get-started:part2
Unable to find image 'gordon/get-started:part2' locally
part2: Pulling from gordon/get-started
10a267c67f42: Already exists
f68a39a6a5e4: Already exists
9beaffc0cf19: Already exists
3c1fe835fb6b: Already exists
4c9f1fa8fcb8: Already exists
ee7d8f576a14: Already exists
fbccdcced46e: Already exists
Digest: sha256:0601c866aab2adcc6498200efd0f754037e909e5fd42069adeff72d1e2439068
Status: Downloaded newer image for gordon/get-started:part2
 * Running on http://0.0.0.0:80/ (Press CTRL+C to quit)
```

No matter where docker run executes, it pulls your image, along with Python and all the dependencies from requirements.txt, and runs your code. It all travels together in a neat little package, and you don’t need to install anything on the host machine for Docker to run it.


## Recap
```
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```
