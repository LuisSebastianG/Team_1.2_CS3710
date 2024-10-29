# Docker Know It All

## **Table of Contents**

### Prerequisites

### Building Docker Image

### Running Docker Image

### Docker Commands What They Are And What They Do






### Prerequisites

1. Make sure you have Docker Desktop installed
   If it's not installed click [here](https://www.docker.com/products/docker-desktop/) and install
2. If you're on a windows machine make sure that you have wsl installed
   To check if it's already installed head to your terminal and type the following
   ```batch
   wsl --version
   ```
   If wsl is installed you should see the version that is on your system
   If it is not installed please head [here](https://learn.microsoft.com/en-us/windows/wsl/install) and install it



### Building Docker Image

1. Create a directory in your local system **NOT IN A CLOUD SERVICE LIKE ONEDRIVE** this is where your container image will be stored
2. Download folder containing Rails environment, place in the directory you just made
3. Using your terminal go to the directory you made
   To see what directory you are in
   ```batch
   ls
   ```
   To head to directory
   ```batch
   cd [directory]
   ```
4. After getting in the directory you made Type the following commands
   ```batch
   docker buildx -t [dockerUserName]/[image name] .
   ```
   or
   ```batch
   docker build -t [dockerUserName]/[image name] .
   ```
   docker buildx build provides advance features for building Docker images
   **DON'T FORGET THE PERIOD AT THE END**


### Running Docker Image

1. Now create another directory to map the docker container files to your local machine. Again **DO NOT USE THE DIRECTORY YOU CREATED ABOVE AND DO NOT PLACE IN CLOUD FOLDER LIKE ONEDRIVE!!**
2. Now using the terminal head to the new directory like like instructed above
3. Once in the directory run one of the following programs that is for your OS
   Windows WSL 2, Mac/Linux Terminal or Git Bash
   ```batch
   docker run -it -p 3000:3000 -v $(pwd):/workspace [dockerUserName]/[image name]
   ```

   Windows PowerShell
   ```batch
   docker run -it -p 3000:3000 -v ${PWD}:/workspace [dockerUserName]/[image name]
   ```
   
   Windows Command Prompt (cmd) - not recommended unless you are good with commands
   ```batch
   docker run -it -p 3000:3000 -v %cd%:/workspace [dockerUserName]/[image name]
   ```
4. Docker run creates a new instance of the image in another container it **WILL NOT REOPEN THE SAME CONTAINER**
5. You are now in the container workspace you can verify you are if there is a # to the very left



### Docker Commands What They Are And What They Do

1. Now to make sure everything is running properly check the ruby version
   Type this command in
   ```batch
   ruby --version
   ```
   Should return ruby version
2. If you type irb as a command it takes you to an interactive ruby environment type quit to exit
3. ```batch
   docker ps
   ```
   This returns a list of the currently running containers
4. ```batch
   docker stop <container_id_or_name>
   ```
   This stops the container you listed
5. ```batch
   docker restart <container_id_or_name>
   ```
   This restarts the listed container (This is how you restart a container you've already made)
6. ```batch
   docker exec -it <container_id_or_name> /bin/sh
   ```
   This enters you to the container workspace and you're ready to start using the container!
   
