# Installation

  ## Build the Jenkins BlueOcean Docker Image (or pull and use the one I built)
  ```
  docker build -t myjenkins:1.0 .
  ```
  ## Create the network 'jenkins'
  ```
  docker network create jenkins
  ```

  ## Run the Container
  
  ### 1.  MacOS / Linux
    ```
    docker run --name jenkins-blueocean --restart=on-failure --detach \
      --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
      --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
      --publish 8080:8080 --publish 50000:50000 \
      --volume jenkins-data:/var/jenkins_home \
      --volume jenkins-docker-certs:/certs/client:ro \
      myjenkins:1.0
    ```
  
  ### 2.  Windows
    ```
    docker run --name jenkins-blueocean --restart=on-failure --detach `
      --network jenkins --env DOCKER_HOST=tcp://docker:2376 `
      --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 `
      --volume jenkins-data:/var/jenkins_home `
      --volume jenkins-docker-certs:/certs/client:ro `
      --publish 8080:8080 --publish 50000:50000 myjenkins:1.0
    ```
  
  ## Get the Password
    ```
    docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
    ```
  
  ## Connect to the Jenkins
    ```
    https://localhost:8080/
    ```
  
  ## Installation Reference:
    https://www.jenkins.io/doc/book/installing/docker/
  
  
  ## alpine/socat container to forward traffic from Jenkins to Docker Desktop on Host Machine
  
  https://stackoverflow.com/questions/47709208/how-to-find-docker-host-uri-to-be-used-in-jenkins-docker-plugin
  
  ```
  docker run -d --restart=always -p 127.0.0.1:2376:2375 --network jenkins -v /var/run/docker.sock:/var/run/docker.sock alpine/socat tcp-listen:2375,fork,reuseaddr unix-connect:/var/run/docker.sock
  ```

  ## To Check Dokcer container details
  ```
  docker inspect <container_id> | grep IPAddress
  ```


# Create docker images from docker container and push to docker hub

  docker commit <container_id> your-docker-hub-username/your-image-name:tag
  ###  Ste-1 commit latest container changes
    ```
    docker commit 90bb807b5928 husssainshahzad/jenkins:1.0
    ```
  ###  Login to docker hub
    ```
    docker login
    ```
  ###  Push latest docker image
    ```
    docker push husssainshahzad/jenkins:1.0
    ```

#  Setup jenkins using docker container

  1.  ##  Pull docker image
      ```
      docker push husssainshahzad/jenkins:1.0
      ```
  2.  ##  Run below command to start jenkins
      ```
      docker run --name jenkins-blueocean --restart=on-failure --detach \
        --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
        --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
        --publish 8080:8080 --publish 50000:50000 \
        --volume jenkins-data:/var/jenkins_home \
        --volume jenkins-docker-certs:/certs/client:ro \
        husssainshahzad/jenkins:1.0
      ```
  3.  ## Use below credential to login
           [Login To Jenkins](http://localhost:8080/)
           -  username:  shahzad
           -  password:  shahzad

