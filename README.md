# Présentation du projet

  
The goal of this project is to create micro-services from the source code that is provided to be able to containerize an application with two modules, a front-end and a back-end. The front-end is a basic php web server with a back-end, an api Our role is to produce the dockerfile, the docker-compose allowing us to deploy this application serenely.

# Architecture 

  
In this project, we will deploy the following architecture:

# Mise en place de l'architecture
# Lancement du VM

On lance de déploiement des VM à l'aide de Vagrantfile.

    vagrant up --provision
 

 - Une fois la VM installée , on se connecte sur la machine via SSH.
  `ssh vagrant@Ip_de_la_machine`(mot de passe c'est vagrant)
  
# Application

The application that you will be working on is named "_student_list_", this application is very basic and enables POZOS to show the list of the student with their age.

student_list has two modules:

-   the first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
-   The second module is a web app written in HTML + PHP who enable end-user to get a list of students

Our work is to build one container for each module an make them interact with each other
Now it is time to explain you each file's role:
 **docker-compose.yml:** to launch the application (API and web app)
 **Dockerfile:** the file that will be used to build the API image (details will be given)
 **student_age.json:** contain student name with age on JSON format
 **student_age.py:** contains the source code of the API in python
  **index.php: PHP** page where end-user will be connected to interact with the service to - list students with age. 
 - You need to update the following line before running the website container to make  _**api_ip_or_name**_  and  _**port**_  fit your deployment  
 url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';`
 -  Using the Dockerfile, we create an image and its GUI in the form of a docker container. The latter listens on ports 8000 and 5000 respectively 
  `vi Dockerfile`
 -   We copy the contents of the provided file into this file.
 - Build your image and try to run it (don't forget to mount  _student_age.json_  file at  _/data/student_age.json_  in the container), check logs and verify that the container is listening and is ready to answer
 `docker build -t  api .`   
 docker run --name myapi -d -t  -p 8000:5000    -v  ${PWD}/student_age.json:/data/student_age.json  api`
Run this command to make sure that the API correctly responding (take a screenshot for delivery purpose)

`curl -u toto:python -X GET http://<host IP>:<API exposed port>/pozos/api/v1.0/get_student_ages`

**Congratulation! Now you are ready for the next step (docker-compose.yml)**
