docker-symfony
==============

[![Build Status](https://secure.travis-ci.org/eko/docker-symfony.png?branch=master)](http://travis-ci.org/eko/docker-symfony)

Juste un petit Docker afin d'avoir une pile complète pour l'exécution de Symfony dans des conteneurs Docker
# Installation

En premier, cloner le dépôt via github ou bitbucket:

```bash
$ git clone git@github.com:matsomm/docker-symfony.git
```

Ensuite, mettre votre application Symfony dans ` dossier symfony` et ne pas oublier d'ajouter ` symfony.dev` dans votre `/ etc / hosts` .

Ensuite, exécuter:

```bash
$ docker-compose up
```

Vous avez terminé, vous pouvez visiter votre application Symfony à l'URL suivante: `http://symfony.dev` 
(accès à Kibana à l'URL suivante : `http://symfony.dev:81`)

_Note :_ vous pouvez reconstruire toutes les images Docker en exécutant :

```bash
$ docker-compose build
```

# Comment ça fonctionne ? 

Voici les images `docker-compose` :

* `application`: Ceci est le conteneur de code de l'application Symfony ,
* `db`: Ceci est le conteneur de base de données MySQL ( peut être changée pour PostgreSQL dans `docker-compose.yml`),
* `php`: Cet élément parent renferme PHP- FPM dans lequel le volume d'application est monté,
* `nginx`: Ceci est le conteneur de serveur web Nginx dans lequel le volume d'application est monté,
* `elk`: Ceci est un récipient de pile ELK qui utilise Logstash pour collecter les journaux, les envoyer dans ElasticSearch et de les visualiser avec Kibana .

Il en résulte dans les récipients de fonctionnement suivants:

```bash
> $ docker-compose ps
        Name                      Command               State              Ports
        -------------------------------------------------------------------------------------------
        docker_application_1   /bin/bash                        Up
        docker_db_1            /entrypoint.sh mysqld            Up      0.0.0.0:3306->3306/tcp
        docker_elk_1           /usr/bin/supervisord -n -c ...   Up      0.0.0.0:81->80/tcp
        docker_nginx_1         nginx                            Up      443/tcp, 0.0.0.0:80->80/tcp
        docker_php_1           php5-fpm -F                      Up      9000/tcp
```

# lecture des logs

Yvous pouvez accéder aux journaux de Nginx et Symfony dans les répertoires suivants dans votre machine hôte :

* `logs/nginx`
* `logs/symfony`

# Utilisation de Kibana!

Vous pouvez visualiser les logs Nginx & Symfony à cette URL : `http://symfony.dev:81`.
