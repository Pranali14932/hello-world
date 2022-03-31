Prerequisites

  maven, Docker plugins should be installed in Jenkins.
  Configure specific version of maven in Jenkins Global Tool Configuration
  Store dockerhub credential in Jenkins credentials with type Username with password and give a unique id as docker_credential
  
 Jenkins file 
  
  In the tools block we have used maven definition to refer the maven installation maven-3.6.3 configured in Jenkins Global tool configuration.

  In the environment block we have created two environment variables DATE and TAG The TAG environment varaible is used the tag the generated docker image.

  In the stages block we have created four stages Build, Docker Build, Pushing Docker Image to Dockerhub and Deploy.

  In the Build stage we are executing mvn clean package command to compile and package the java application.

  In the Docker Build stage we have used docker plugin to build the docker image
  
  In the Pushing Docker Image to Dockerhub we are pushing the docker images with two tags unique tag(${TAG}) and the latest tag to dockerhub using credentials stored in   Jenkins credentials
      First we are pushing the docker image with unique tag(${TAG}) to the dockerhub
      Second we are retagging the generated docker image to latest tag and pushing to dockerhub.Tagging every image to latest tag, will help in get the latest changes         everytime.
     
  In the Deploy stage we are creating the docker container with the latest docker image in the same Jenkins server.
  
  Inside the docker container we have tomcat, by default tomat is exposed on port 8080. We are mapping the port 9004 of the Jenkins server to the port 8080 of docker       container.

When we access the application by http://jenkins-server-ip-address:9004/hello-world, the 9004 will be port-forwarded to 8080 of the docker container
