# JenkinsAngular

0 Add the file nginx.conf, Dockerfile and docker-compose.yml to the project, also add the next command to package.yml field: 

"build:prod": "ng build --prod --build-optimizer",

1 Register the aws credentials in Publish ssh Plugin (Host, Username, pem file)

2 In the ec2 instance, enable docker registry with : 
docker run -d -p 5000:5000 --restart=always --name registry registry:2

3 Create a new project of jenkins, add the git repo and add a shell step. In this step add the commands listed bellow:

docker build -t jenkins-angular:latest .
docker tag jenkins-angular:latest ec2-18-222-85-65.us-east-2.compute.amazonaws.com:5000/jenkins-angular
docker push ec2-18-222-85-65.us-east-2.compute.amazonaws.com:5000/jenkins-angular

4 Add the publish sh plugin and specify the aws credentials added before.

5 Specify the files to be copied, in this case the docker-composer.yml file 

6 Add the commands to be executed on the ec2 instance in the Exec Command field: 

docker-compose down
docker image rm jenkins-angular
docker pull localhost:5000/jenkins-angular
docker tag localhost:5000/jenkins-angular jenkins-angular
docker image rm localhost:5000/jenkins-angular
docker-compose up -d
