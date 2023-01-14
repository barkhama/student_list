Présentation du projet
Le but de ce projet est créer des micro-services à partir du code source qui est fourni pour pouvoir conteneuriser une application avec deux modules un front-end et un back-end.
Le front-end c’est un serveur web basic php avec un back-end , une api
Notre role c’est de produire le dockerfile, le docker-compose permettant de pouvoir déployer cette application sereinement .

Architecture
Dans le présent projet , nous allons déployer l’architecture suivante:

Mise en place de l’architecture
Lancement du VM
On lance de déploiement des VM à l’aide de Vagrantfile.

vagrant up --provision
Une fois la VM installée , on se connecte sur la machine via SSH.
ssh vagrant@Ip_de_la_machine(mot de passe c’est vagrant)

A l’aide du Dockerfile, on crée une image et son IHM sous forme de conteneur docker. Ce dernier écoute respectivement sur les ports 8000 et 5000
vi Dockerfile

On copie le contenu du fichier fourni dans ce fichier.
Pour générer l’image docker , on fait un build avec docker build

docker build -t api .

Pour tester notre conteneur on fait un docker run

docker run --name myapi -d -t -p 8000:5000 -v ${PWD}/student_age.json:/data/student_age.json api
