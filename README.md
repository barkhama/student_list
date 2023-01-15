  
# Project display

  
The goal of this project is to create micro-services from the source code that is provided to be able to containerize an application with two modules, a front-end and a back-end. The front-end is a basic php web server with a back-end, an api Our role is to produce the dockerfile, the docker-compose allowing us to deploy this application serenely.

# Architecture 

  
In this project, we will deploy the following architecture:

#   Implementation of the architecture
#   Launching the VM
 We launch VM deployment using Vagrantfile.
 
    vagrant up --provision

 - Once the VM is installed, connect to the machine via SSH.
  `ssh vagrant@Ip_de_la_machine`(password is vagrant)
  
# Application

The application that you will be working on is named "_student_list_", this application is very basic and enables POZOS to show the list of the student with their age.

student_list has two modules:

-   the first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
-   The second module is a web app written in HTML + PHP who enable end-user to get a list of students

Our work is to build one container for each module an make them interact with each other
Now it is time to explain you each file's role:
 

 - docker-compose.yml: to launch the application (API and web app)
 - Dockerfile: the file that will be used to build the API image (details will be given)
 -  student_age.json: contain student name with age on JSON format
 - student_age.py: contains the source code of the API in python
 - index.php: PHP page where end-user will be connected to interact with the service to - list students with age. 
 - We need to update the following line before running the website container to make  _**api_ip_or_name**_  and  _**port**_  fit our deployment  
 url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages'.
   ## Build and test our API Image
   Using the Dockerfile, we create the API image and its IHM in the form of a docker container. The latter listens on ports 8000 and 5000 respectively 
  `vi Dockerfile`
 We copy the contents of the provided file into this file.
 Build our image and try to run it (don't forget to mount  _student_age.json_  file at  _/data/student_age.json_  in the container), check logs and verify that the container is listening and is ready to answer
 `docker build -t  api . and after 
     docker run --name myapi -d -t  -p 8000:5000    -v  ${PWD}/student_age.json:/data/student_age.json  api`

We run this command to make sure that the API correctly responding 

`curl -u toto:python -X GET http://@Ip_Server:8000/pozos/api/v1.0/get_student_ages`

**Congratulation! Now we are ready for the next step (docker-compose.yml)**
    

##  Infrastructure As Code

After testing our API image, we need to put all together and deploy it, using docker-compose.
The  _**docker-compose.yml**_  file will deploy two services :
-   website: the end-user interface with the following characteristics
    -   image: php:apache - environment: we will provide the USERNAME and PASSWORD to enable the web app to access the API through authentication
    -   volumes: to avoid php:apache image run with the default website, we will bind the website given by POZOS to use. We must have something like  `./website:/var/www/html`
    -   depend on: we need to make sure that the API will start first before the website
    -   port: do not forget to expose the port
-   API: the image builded before should be used with the following specification
    -   image: the name of the image builded previously - volumes: We will mount student_age.json file in /data/student_age.json
    -   port: don't forget to expose the port

We copy the contents of the provided file into this file.
**NB:** Delete your previous created container

    docker rm name_container

Now we can run our docker-compose.yml

    docker-compose up -d
Finally, we can access our application and click on the bouton "List Student"

    http://@IP_Server:8080

**If the list of the student appears, we are successfully dockerizing the POZOS application! Congratulation (make a screenshot)**
