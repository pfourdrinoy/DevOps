## TP Docker

#### Instructions Docker
Créer le dockerfile
Il sert à initialiser les base de notre environnement Docker, notamment le système d'exploitation mais aussi des variables d'environnement.
Il faut ensuite construire cette image avec la commande dans mon fichier:

1. **Construire l'image PostgreSQL :**
   ```bash
   docker build -t mon-postgres:14.1 .
   ```
Nous souhaitons ensuite accèder notre bdd postgres via adminer. Pour cela il faut que les deux containers puissent communiqué l'un avec l'autre. Il est donc nécessaire de les mettre sur le même network.
On procède à la création du network via cette commande:

2. **Créer un réseau Docker :**
   ```bash
   docker network create app-network
   ```

Il faudra alors lancer nos container dans ce network en rajoutant l'options --net:app-network
Enfin, pour garantir la persistance de nos fichiers, il nous faut rajouter dans l'executable dockerfile la ligne suivante:

```COPY ./data /docker-entrypoint-initdb.d
```

Cette ligne permettra de garantir qu'à chaque création de container les tables et les données de base soient ingérées.
Toujours dans l'idée d'assurer la persistance de nos fichiers, il faut mount les fichiers pour garder les données nouvellements ajouté:
Nous avons finalement ces deux commandes d'initialisation:

3. **Lancer Adminer et PostgreSQL :**
   ```bash
   docker run --name mon_adminer -p 8080:8080 --network app-network -d adminer
   docker run --name mypostgres -p 5432:5432 -d --network app-network -v ./data:/docker-entrypoint-initdb.d mon-postgres:14.1
   ```

---

## Backend

#### Instructions Docker

1. **Construire l'image de l'API :**
   ```bash
   docker build -t mysimpleapi .
   ```

2. **Lancer l'API :**
   ```bash
   docker run --name MySimpleAPI -p 80:8080 -d mysimpleapi
   ```

---

### Pourquoi un build en plusieurs étapes (multistage build) ?

Un build multistage dans Docker est essentiel pour :
1. **Efficacité** : Il sépare l'environnement de build de l'environnement d'exécution final, optimisant ainsi la taille et la vitesse de construction.
2. **Sécurité** : Réduit la surface d'attaque en excluant les dépendances de build de l'image finale.
3. **Réduction de la taille** : Assure que l'image finale contient uniquement les composants nécessaires à l'exécution, minimisant ainsi le superflu.

---

### Pourquoi avons-nous besoin d'un reverse-proxy ?

Un reverse proxy est nécessaire pour :
- **Améliorer les performances** en répartissant les requêtes entre les serveurs.
- **Renforcer la sécurité** en protégeant les serveurs backend et en optimisant l'utilisation des ressources.
- **Centraliser la surveillance** et la journalisation, supporter des protocoles modernes comme HTTP/2, et assurer une livraison efficace du contenu aux clients.

---

Le but de ce TP était en définitive de créer notre application 3 tier de manière à ce qu'elle soit implementable rapidement dans n'importe quel environnement avec docker. En une seule commande avec docker compose, vous avez une Base de donnée créer que vous pouvez consulter via une API elle même protéger par un reverse-Proxy. Chaque application fonctionant dans un container spécial et pouvant ainsi être gérer séparemment.

---

## TP GITAction

```bash
git init
git add .
git commit -m "Rendu TP1"
git remote add origin https://github.com/pfourdrinoy/DevOps
git push -u origin master
```

---

## Qu'est-ce que Testcontainers ?

Testcontainers est une bibliothèque Java permettant d'exécuter des conteneurs Docker pendant les tests d'intégration, facilitant ainsi l'intégration de l'application avec des services comme PostgreSQL.

---

## Création du workflow

```bash
mkdir -p .github/workflows
New-Item -ItemType File -Path .github/workflows/main.yml
```
---

Le but de ce TP était de créer un workflow permetant d'effectuer de nombreuses tâches à chaque fois que l'on push notre code sur github. Les possibilités sont infinis, mais il est notamment très intéressant de d'ajouter des tests de qualité de code tels que maven ou alors des actions d'upload de nos images en temps réel. C'est ce que nous avons décidé de faire. A chaque push, notre code va être évalué par sonar et maven pour limiter la dette technique et nous informé en temps réel de cette dernière. Une fois cette étape réalisé, les images dockers de nos applications sont modifiés sur docker hub automatiquement. C'est de l'intégration en continue. De plus, pour assurer la sécurité de nos comptes, nous avons utiliser des variables d'environnement configurés directement sur notre compte github.

## TP Ansible

Connection et ping au serveur:
```bash
ssh -i ~/.ssh/id_rsa centos@paul.fourdrinoy.takima.cloud
ansible all -i /mnt/c/Users/paulf/Documents/EPF/DevOps/ansible/inventories/setup.yaml -m ping
```

Lancement du playbook et implémentation de l'app sur le serveur
```bash
ansible-playbook -i /mnt/c/Users/paulf/Documents/EPF/DevOps/ansible/inventories/setup.yaml /mnt/c/Users/paulf/Documents/EPF/DevOps/ansible/playbook.yaml
```

Le but de ce TP était de configurer le système ansible, permetant de déployer par une simple commande une application sur plusieurs serveurs et ce en parallèle sans avoir ajouté d'application auparavant sur ce serveur.

---