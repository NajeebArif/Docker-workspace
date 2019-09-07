# Dockerfile for extending jboss/wildfly

This docker file is an extension to [Jboss Wildfly](https://hub.docker.com/r/jboss/wildfly)

## How to use this docker file:
  First build the image using docker build command and provide any tag which you want.
  
  `docker build -t anytag-wildfly-with-admin-console .`
  
  > you can change the user name and password in the dockerfile and build the image by default they are 'admin' and 'Admin#70365'
  
  Once the image is created you can use it in your applications to deployment.
  
## Deploying Thin WAR files
  To deploy thin war files you can create a `Dockerfile` in you projects root directory as:
  
  ```
  FROM anytag-wildfly-with-admin-console

  LABEL maintainer="najeeb.oo7.dd@gmail.com"

  COPY your-app.war $DEPLOYMENT_DIR
  ```
  
  Now run the docker build once again
  
  `docker build -t image-name .`
  
  After this image is built you can ship it. >(Probably push it to the docker hub)
  
  Now you just have to run this image using:
  
  `docker run -it --name name-of-the-container -p 8080:8080 -p 9990:9990 image-name`
  
  or to specify the memory and heap size
  
  `docker run -m 1G -e JVM_ARGS="-Xmx512M" -it --name name-of-the-container -p 8080:8080 -p 9990:9990 image-name`
  
  To verify if the specified memory and heap size was set properly you can connect to your running docker container and then check the flags of the wildfly process running in it.
  
  ```
  docker exec -ti <container-name> /bin/bash 
  ..
  jps
  ..
  jinfo -flags <pid-which-you-got-from-running-java-process>
  ```
  
  You can now access the admin console at `localhost:9990` with the credentials defined and the deployed app will be exposed on port `localhost:8080`.
