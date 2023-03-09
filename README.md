# TP1-docker

5 A docker pull httpd /// pour 
5 B docker ls image /// pour lister les images
5 D docker run -d -p 8080:80 -v C:/projets/tpdocker/tp1-docker/src/index.html:/usr/local/apache2/htdocs/index.html httpd
    docker run -p 8080:80 --name sus2 -d -v C:/projets/tpdocker/tp1-docker/index.html:/usr/local/apache2/htdocs/index.html httpd

5 E il faut dabord créer un contenair avant d'y copier notr htmml avec cp:
    *docker run -d -p 80:80 --name my-apache httpd:latest
    *docker cp index.html my-apache:/usr/local/apache2/htdocs/

6 A voici le contenu du dockerfile qui va créer une image pour un serveur web apache:
FROM httpd:latest
COPY src/index.html /usr/local/apache2/htdocs/

6 B il faut faaire docker build . -t benze_image puis docker run --name benze -d -p 8080:80 benze_image
6 C La principale différence entre les procédures 5 et 6 est que la procédure 5 utilise un volume pour partager le fichier index.html entre le système d'exploitation hôte et le conteneur, tandis que la procédure 6 utilise un Dockerfile pour inclure le fichier index.html directement dans l'image.

L'avantage de la procédure 5 est que vous pouvez modifier le fichier index.html à tout moment sur le système d'exploitation hôte, et les modifications seront immédiatement visibles dans le conteneur. Cela permet une grande flexibilité lors du développement de votre application.

L'avantage de la procédure 6 est que l'image Docker contient toutes les dépendances et configurations nécessaires pour exécuter votre application. Cela rend votre application plus portable et facile à déployer sur différents environnements.

7 A     on récupère les images avec ces commandes:
docker pull mysql:5.7
docker pull phpmyadmin/phpmyadmin

7 B nous allons avoir besoin d'un réseau pour connecter les deux conteneur:
    docker network create my-network

    pour le conteneur MySQL:
    docker run -d --name mysql-container \
    -e MYSQL_ROOT_PASSWORD=randompass \
    -e MYSQL_DATABASE=my_database \
    --network my-network \
    mysql:5.7

    pour php my admin:
    docker run -d --name phpmyadmin-container \
    -e PMA_HOST=mysql-container \
    -p 8080:80 \
    --network my-network \
    phpmyadmin/phpmyadmin

    une fois les deux conteneur lancé il suffit d'aller sur le port 8080 pour accéder à phpmyadmin et y rentrer des donnés avec des requetes sql