Example of Mutual Client Authorization in SpringBoot.

This repository was forked from forked from joutwate/mtls-springboot, and adds to it:
- support for running the client app in a dedicated docker instance
- support for running the server app in a dedicated docker instance

The apps were changed following the advices taken from https://spring.io/guides/gs/spring-boot-docker/

This README contains only the part that is relative to the work necessary to run the demo on docker. For a complete documentation on the solution, please see the README file of the parent repository.

**Overview**
As in its parent repository, this repository contains 2 spring boot applications that connects to each other requiring mutual authentication. 

**Preprequisites**
- maven
- docker. For more info, please check https://www.docker.com/

**Quickstart**

The following quickstart makes use of keys, keystores and trustStore that were already generated and committed to the repo. For more information on how the keyStore and trustStore were generated, please check the file gen-non-prod-key.sh under the bin directory.

```
cd mtls-springboot/server
# create the docker image for the server
mvn install dockerfile:build
#run the server
docker run -p 8111:8111 ermannopirotta/ssl-mutual-authentication-server
# In another shell
cd mtls-springboot/client
#create the docker image for the client
mvn install dockerfile:build
#Upon creation the test which connects via ssh to the server is executed. Thus, if the image is created we can be sure that the mutual authentication between the 2 apps works. For testing it in the browser, see the 'test in the browser' section
```
**Test in the browser**

Both server and client applications expose 2 https endpoint which can be used to test ssl mutual authentication between the browser (the client) and the apps (the server).

The following are the steps required to test the client application. The server application can be tested in a similar way.
- start the client application in docker with docker run -p 8222:8222 ermannopirotta/ssl-mutual-authentication-client
- open Firefox -> Preferences -> Privacy and Security
- under "Certificates", click con "View Certificates"
- select the "Your Certificates" tab and click on "Import"
- select the mtls-springboot/client/src/main/resources/client.p12 file
- navigate to https://localhost:8222/client/
- when prompted, accept the "server" ssh certificate
- when prompted, provide the client ssh certificate
- you should be presented with a white page with the label: "Client successfully called!"
