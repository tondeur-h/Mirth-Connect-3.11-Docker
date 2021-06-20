## Builder l'image  
```  
docker build -t mirth311 .  
```  
## Fichier Docker-Compose  

```
version: "3.1"
services:
  mc:
    image: mirth311
    environment:
      - DATABASE=postgres
      - DATABASE_URL=jdbc:postgresql://db:5432/mirthdb
      - DATABASE_MAX_CONNECTIONS=20
      - DATABASE_USERNAME=mirthdb
      - DATABASE_PASSWORD=mirthdb
      - VMOPTIONS=-Xmx512m
    volumes:
      - ./data/volumes/appdata:/opt/connect/appdata
      - ./data/volumes/hl7:/opt/hl7
    ports:
      - 8080:8080/tcp
      - 8443:8443/tcp
    depends_on:
      - db
  db:
    image: postgres
    environment:
      - POSTGRES_USER=mirthdb
      - POSTGRES_PASSWORD=mirthdb
      - POSTGRES_DB=mirthdb
    expose:
      - 5432
```

# Options mirth.properties

## BASE DE DONNÉES
Type de base de données à utiliser pour la base de données principale de NextGen Connect Integration Engine. 

***Options:***  
mysql
postgres

**La database est impérative pour la persistence du pâramètrage du service mirth connect.  
Dans ce projet à été fait le choix de PostGresQl sur le port 5432, il faudra peut être ajuster ce port si vous voulez utiliser 2 bases de données postgresql différentes, sinon ajuster les profils postgres si vous voulez mutualiser.**   

**DATABASE_URL**   
L'URL JDBC à utiliser lors de la connexion à la base de données. 

***Par exemple:***  
jdbc:postgresql://serverip:5432/mirthdb

**DATABASE_USERNAME**  
Le nom d'utilisateur à utiliser lors de la connexion à la base de données. Si vous ne souhaitez pas utiliser une variable d'environnement pour stocker des informations sensibles comme celle-ci, consultez la section Utilisation des secrets Docker ci-dessous.


**DATABASE_PASSWORD**  
Le mot de passe à utiliser lors de la connexion à la base de données. Si vous ne souhaitez pas utiliser une variable d'environnement pour stocker des informations sensibles comme celle-ci, consultez la section Utilisation des secrets Docker ci-dessous.


**DATABASE_MAX_CONNECTIONS**  
Nombre maximal de connexions à utiliser pour le pool de connexions du moteur de messagerie interne.


**VMOPTIONS**  
Liste des options de ligne de commande JVM séparées par des virgules à placer dans le fichier .vmoptions. Par exemple, pour définir la taille maximale du tas:

***-Xmx512m***  

# Notion de partage volumes  

```
volumes:
      - ./data/volumes/appdata:/opt/connect/appdata
      - ./data/volumes/hl7:/opt/hl7
```  

***2 partages sont mis en place :***  
- Le premier pour la persistence database mirth connect  
- Le second pour les échanges de fichiers local avec le container   


![partage Volume](https://github.com/tondeur-h/Mirth-Connect-3.11-Docker/blob/c2cded03cb3169d991c76d0bdf966f8e5ab5fc8a/images/partage_container.png "partage volume")  

# Accéder au service via le client Mirth Connect administrator 1.2   

[Mirth Connect administrator Launcher](https://www.nextgen.com/products-and-services/nextgen-connect-integration-engine-downloads)  

Télécharger la version du client correspondant a votre OS client    

## Premier démarrage  

Installer et exécuter Mirth Connect Administrator Launcher, parametrer comme ci dessous :  

![Dialogue du Launcher](https://github.com/tondeur-h/Mirth-Connect-3.11-Docker/blob/c2cded03cb3169d991c76d0bdf966f8e5ab5fc8a/images/mirth_administrator.png "Dialogue du Launcher")  

Au premier démarrage il est demandé le login et mot de passe par défaut de l'installation =>  admin / admin   

Puis il est demandé de changer ce mot de passe du compte admin => renseigner les éléments nécessaires  

[Connect User Guide](https://www.nextgen.com/-/media/files/nextgen-connect/nextgen-connect-311-user-guide.pdf)  

# FIN  :-) 
