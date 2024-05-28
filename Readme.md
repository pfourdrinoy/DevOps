Créer le dockerfile
 Il sert à initialiser les base de notre environnement Docker, notampment le système d'exploitation mais aussi des variables d'environnement.
 Il faut ensuite construire cette image avec la commande dans mon fichier:

docker build -t mon-postgres:14.1 . 

Nous souhaitons ensuite accèder notre bdd postgres via adminer. Pour cela il faut que les deux containers puissent communiqué l'un avec l'autre. Il est donc nécessaire de les mettre sur le même network.
On procède à la création du network via cette commande:

docker network create app-network

Il faudra alors lancer nos container dans ce network en rajoutant l'options --net:app-network
Enfin, pour garantir la persistance de nos fichiers, il nous faut rajouter dans l'executable dockerfile la ligne suivante:

COPY ./data /docker-entrypoint-initdb.d

Cette ligne permettra de garantir qu'à chaque création de container les tables et les données de base soient ingérées.
Toujours dans l'idée d'assurer la persistance de nos fichiers, il faut mount les fichiers pour garder les données nouvellements ajouté:
Nous avons finalement ces deux commandes d'initialisation:

docker run --name mon_adminer -p 8080:8080 --network app-network -d adminer
docker run --name mypostgres -p 5432:5432 -d --network app-network -v ./data:/docker-entrypoint-initdb.d mon-postgres:14.1

Backend

Au départ il était compliqué d'utiliser docker en installant le JDK. Ce n'etait pas la façon la plus optimale de le faire. Ensuite nous avons créer un docker afin de lancé seulement une fois le programme dans le docker

En utilisant un build multi-étape, nous n'incluons que les artefacts nécessaires à l'exécution dans l'image finale. Cela permet de réduire la taille de l'image. De plus, l'utilisation de Docker pour la construction et l'exécution, nous permet de garntir un environnement de construction et un environnement d'exécution cohérent. Il n'est donc plus nécessaire d'installer le JDK pour pouvoir lancé son container et build son image

docker build -t mysimpleapi .
docker run --name MySimpleAPI -p 80:8080 -d mysimpleapi

Nous avons donc réussis à créer une API fonctionnelle permettant de faire des requetes à notre base de donnée postgres.
docker build -t apimain .
docker run --name APIMain --network app-network -p 80:8080 apimain


docker build -t http .
docker run --name HTTP -p 80:80 --network app-network http


TP

GITAction

git init
git add .
git commit -m "Rendu TP1"
git remote add origin https://github.com/pfourdrinoy/DevOps
git push -u origin master


mkdir -p .github/workflows
New-Item -ItemType File -Path .github/workflows/main.yml
