## Docker 

Docker is a platform for containerization, a method of packaging and running applications and their dependencies in isolated environments known as containers. Some important concepts are:

- **Containerization**: Docker uses containerization to encapsulate applications and their runtime environments into self-sufficient units called containers. These containers include the application code, libraries, and configurations, ensuring consistent and reproducible runtime environments.

- **Portability**: Docker containers are highly portable because they are abstracted from the underlying host system. They run consistently across different environments, whether it's a developer's laptop, a test server, or a production cluster.

- **Resource Efficiency**: Docker containers share the host operating system's kernel, resulting in minimal overhead and efficient resource utilization. This allows you to run multiple containers on a single host, optimizing resource allocation.

- **Version Control**: Docker images are snapshots of containers at specific points in time. These images are versioned, facilitating version control for application components and enabling rollback or recreation of previous states.

- **Scalability**: Docker simplifies application scaling by allowing the rapid deployment of additional containers to handle increased workloads. Container orchestration tools like Kubernetes automate container management, making scaling even more efficient.

- **DevOps & CI/CD**: Docker plays a pivotal role in DevOps practices and CI/CD pipelines. Containers ensure that applications tested in development environments are identical to those deployed in production, reducing deployment risks.

- **Security**: Docker provides security through container isolation mechanisms and capabilities restriction. It also supports signed images and vulnerability scanning tools for enhanced security posture.

- **Ecosystem**: Docker boasts an extensive ecosystem of pre-built images on Docker Hub. It supports multiple programming languages and frameworks, reducing the need to configure infrastructure from scratch.

Docker simplifies application packaging, deployment, and management through containerization, promoting consistency, efficiency, and scalability across the software development lifecycle.


### Prerequisites

Before starting lets ensure that all the prequsities have been met

1. **EC2 Instance:** For this the very first step is to have 2 EC2 machine active, one machine will be our production machine and the other machine will be a docker machine.

2. **Installing Docker:** On the Docker Machine we need to install it. Since the OS is linux, follow this: https://docs.docker.com/engine/install/ubuntu/ . Once installed simply type docker to confirm if it has been installed.

3. **Confirm App gets Deployed Sucesfully:** In the production Machine make sure that node and other necessary dependencies is installed. Have a manual deploymenet first just to confirm everything is working in place.


### Implementing Docker

1. **Starting Docker and adding configuration:** On the Docker Machine clone the repo that has the working builds. Here we need to add nginx configuration. For that we need to run these two steps in the terminal. The 'touch'command will create a file in linux.
   
   ```bash
   touch nginx.conf
   ```
   Inside this nginx.conf add the following content using a text editor such as vim or nano.
   ```bash
   server {
     listen 80;
   
     location / {
       root /usr/share/nginx/html/;
       include /etc/nginx/mime.types;
       try_files $uri $uri/ /index.html;
     }
    }
   ```
2. **Adding Dockerfile:** We need to add the docker file to build the image. The Contents has been given [here](https://github.com/thomasjv799/Devops_Training/blob/main/5_Docker/Dockerfile). Either copy paste or upload the file inside the repo.

3. **Setting up Dockerhub:** Docker Hub is a service provided by Docker for finding and sharing container images. We will pushing our build image to there and for that we need to set a account and repo. Follow the steps [here](https://github.com/thomasjv799/Devops_Training/blob/main/5_Docker/CreateaDockerrepositoryonDockerHub.pdf)

4. **Building Docker Image:** For building our docker image go to the terminal and run:
   ```bash
   docker build -t reactjs-app:v1 .
   ```
   Here this command means as follows

   1. 'docker build': This is the main Docker command for building Docker images. It tells Docker that you want to create a new Docker image. Add sudo in front of the command if you dont have permission.

   2. '-t reactjs-app:v1': This part of the command specifies the name and tag for the Docker image you're building. In this case, it's named "reactjs-app" with the tag "v1". The -t flag is short for --tag, and it allows you to provide a name and optionally a tag for the image.
      * reactjs-app is the name of the image. You can think of this as a label or identifier for the image.
      * v1 is the tag. Tags are used to version your Docker images. In this case, "v1" suggests that this is the first version of your ReactJS application image. Tags are optional but recommended for versioning and managing your images.
        
   3. '.' (period): This is the build context. The build context is the path to the directory where your Dockerfile is located, and it specifies the set of files and directories that Docker will use when building the image. In this case, the Dockerfile is expected to be in the current directory, denoted by the period.
  
   > Warning: If your running in ec2 instance there is a high chance that it could crash as the free tier of ec2 currently gives only 1 GB of RAM. If that is the case,
   > it is advised to install docker locally. In Windows/Mac OS install [Docker Desktop](https://www.docker.com/products/docker-desktop/), then take a terminal, go to where the repo is cloned and run the above docker build command.

5. **Test locally if it runs:** In order to see the list of docker images run the following command:
   ```bash
   docker images
   ```
   You will be able to see the image details here. Once then in order run the image to a container run the following command:
   ```bash
   docker run -d -it -p 1234:80 --name reactjs-app-test reactjs-app:v1
   ```
   The command is explained as below:

     1. `docker run`: This is the main Docker command for running a Docker container. The `run` command is used to start a new container based on a specified Docker image.
  
     2. `-d`: This flag stands for "detached mode." When you run a container in detached mode, it means the container runs in the background, and you can continue to use your terminal for other commands without being attached to the container's console. This is typically used for long-running services.
  
     3. `-it`: These flags are used together. 
     - `-i` stands for "interactive." It allows you to interact with the container's shell or command prompt if needed.
     - `-t` stands for "terminal." It allocates a pseudo-TTY (text terminal) for the container, which is often necessary when running interactive processes.
  
     4. `-p 1234:80`: This flag is used to map network ports between the host machine and the container. It specifies that port 1234 on the host should be mapped to port 80 on the container. In other words, any traffic sent to port 1234 on the host will be forwarded to port 80 in the container. This is commonly used to expose services running in the container to the outside world.
  
     5. `--name reactjs-app-test`: This flag allows you to specify a name for the running container. In this case, the container will be named "reactjs-app-test." Naming containers can make it easier to manage and reference them later.

     6. `reactjs-app:v1`: This is the name and tag of the Docker image that you want to use to create the container. In this case, it's "reactjs-app" with the tag "v1." Docker will use this image as the basis for the container.

  If it is local system go to localhost:1234 or if ec2 machine first allow the traffic to that port and add then use the IP Address followed by the port to see the application.


6. **Pushing to Dockerhub:** Once its running locally we need to push to Dockerhub so that anyone can use it. First we need to align the name of our image to be of same with the hub repo. For that we need to rename it. As we saw in while creating the repo, my username is thomas799 and repo is react-docker. We need to change the name of our local image similarly for that lets change it using the following command:
   
```bash
docker tag reactjs-app:v1 thomas799/react-docker:v1
```
  Make sure to change with your username and repo name along with with tag which in this case is v1.

  Once that is done run this step to push:

```bash
docker push thomas799/react-docker:v1
```

  This will push the image to dockerhub. You can go and check your hub to see the image pushed.

7. Now basically anyone with docker in the system can run the application locally without installing any dependencies at all. In a new system have docker installed and run:
  
```bash
  docker login
```
This is to login to docker hub

Then run
  
```bash
  docker pull thomas799/react-docker:v1
```

This will pull the docker image from the hub

Finally to run it on that system locally
  
```bash
    docker run -d -it -p <PORT>:80 --name reactjs-app-test reactjs-app:v1
```
So using these much commands we can quickly fire up a application very easily. A Docker CI CD pipeline yaml file has been also added for further experimentation.
     
     
