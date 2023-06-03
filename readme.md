# Jenkins - Beginner

## Description

This guide provides an easy way to install and create a simple Jenkins project.

## Installation

### Build the Jenkins BlueOcean Docker Image

```shell
docker build -t myjenkins-blueocean:2.332.3-1 .
```

### Create the network 'jenkins'
```shell
docker network create jenkins
```
## Run the Container
MacOS / Linux

```shell
docker run --name jenkins-blueocean --restart=on-failure --detach \
  --network jenkins --env DOCKER_HOST=tcp://docker:2376 \
  --env DOCKER_CERT_PATH=/certs/client --env DOCKER_TLS_VERIFY=1 \
  --publish 8080:8080 --publish 50000:50000 \
  --volume jenkins-data:/var/jenkins_home \
  --volume jenkins-docker-certs:/certs/client:ro \
  myjenkins-blueocean:2.332.3-1
```

### Get the Password
To get the initial admin password for Jenkins, run the following command:
  
  ```shell  
  docker exec jenkins-blueocean cat /var/jenkins_home/secrets/initialAdminPassword
```

### Connect to Jenkins
Once Jenkins is up and running, you can access it through the following URL:

https://localhost:8080/


## License

This project is licensed under the [MIT License](LICENSE).

## Contact

For any questions or support, please contact [samprince.franklink2020@vitstudent.ac.in](mailto:samprince.franklink2020@vitstudent.ac.in).
