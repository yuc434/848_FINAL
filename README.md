# Big Data Processing Toolbox
General Description:
This is a microservice-based application that would allow the users to run Apache Hadoop, Spark, Jupyter Notebooks, SonarQube and SonarScanner without having to install any of them. 

# IPs to try the published services:
GUI (You can access this link to see the final toolbox):
http://35.192.176.97/

Hadoop:
http://34.136.45.207:9870/

Spark: 
http://34.134.37.101:8080/

jupyter notebook: 
http://34.66.58.39:8888/

Sonarqube and Sonar Scanner:
http://34.135.109.234:9000/

# Docker Images used for this project:
Hadoop:
- Masternode: https://hub.docker.com/r/bde2020/hadoop-namenode
- Datanode: https://hub.docker.com/r/bde2020/hadoop-datanode

Spark: 
https://hub.docker.com/r/bitnami/spark/

jupyter notebook: 
https://hub.docker.com/r/jupyter/base-notebook/

Sonarqube and Sonar Scanner:
https://hub.docker.com/r/yuhongc/sonarqube_scanner

GUI:
https://hub.docker.com/r/yuhongc/gui

# Steps to get the application to work:
## 1. Pull this directory
This is a easy one :)
## 2. Push the image of sonarqube_scanner to docker hub
for sonarqube_scanner, run the following commands:
```
cd sonarqube_scanner
docker build -f Dockerfile.test -t $DOCKER_USER_ID/sonar .
docker push $DOCKER_USER_ID/sonar
```

## 3. Go to Google Cloud Platform and set up the project and the cluster
1. create a new project
2. Use Kubernetes API to create a new kubernetes cluster
3. Use GCP cloud shell to pull and tag all images from dockerhub 
    - commands to use (use sonarqube_scanner as an example):
        ```
        - docker pull $DOCKER_USER_ID/sonar
        - docker tag $DOCKER_USER_ID/sonar gcr.io/$GCP_PROJECT_ID/$DOCKER_USER_ID/sonar
        ```
4. push all images to GCP container registry.
    - commands to use (use sonarqube_scanner as an example):
        ```
        - docker push gcr.io/$GCP_PROJECT_ID/$DOCKER_USER_ID/sonar
        ```

## 4. Deploy all services
1. Go to GCP container registry, you should see all images now. 
2. For each image: 
    - click on deploy and choose Deploy to GKE
    - (For Spark, Hadoop namenode and Hadoop datanode only) In the Environment variables section, add the environment variables in the env.md file under the corresponding github directory. 
    - Expose the loadbalancer service on the port specified in the env.md file under the corresponding github directory. 
    - After the service is exposed, you can now access the service via the corresponding Endpoints.

## 5. Change the url in gui to your own IPs, and deploy the toolbox GUI
1. modify the gui/index.html file. Change the IPs of the existing ones to the endpoints you get from the previous step.
```
cd gui
vim index.html  // do the modifications here 
docker build -f Dockerfile.gui -t $DOCKER_USER_ID/gui .
docker push $DOCKER_USER_ID/gui
```
2. deploy the GUI docker image following the same step above.
3. You can now access the toolbox GUI via the Endpoints you just created. GOOD JOB!






