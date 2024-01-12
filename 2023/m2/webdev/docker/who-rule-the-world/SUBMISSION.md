# "WhoRuleTheWorld" - Ryme LEHNA - Ynov Mastère 2 Dev Web

## Sommaire
- Clone du projet dans la machine virtuelle
- Création de private registry
- Vote
- Seed-data
- Result
- Création des images
- Publication des images dans "registry"
- Publication des images sur DockerHub
- Création du Docker compose
- Résultat.

## Clone du projet dans la machine virtuelle

Il suffit de lancer la commande suivante dans la machine virtuelle pour cloner le projet : 
`git clone https://github.com/Rymuu/ynov-resources.git`

## Création du private registry

On lance les commandes suivantes pour la créatioon d'un nouveau private registry pour afin de stocker nos images : 

1. Création du registre exposant sur le port 5000 nommé "registry" : 
`docker run -d -p 5000:5000 --restart always --name registry registry:2`

2. Création d'un network "registry-network" :
`docker network create registry-network`

3. Connexion entre "registry" et  "registry-network":
`docker network connect registry-network registry`

4. J'utilise un front pour le registre exposant sur le port 8000.
`docker run -d -e ENV_DOCKER_REGISTRY_HOST=registry -e ENV_DOCKER_REGISTRY_PORT=5000 --network registry-network -p 8000:80 konradkleine/docker-registry-frontend:v2`

5. J'ajoute le port 8000 dans le réseau de la machine virtuelle : 
![image](https://github.com/Rymuu/ynov-resources/assets/96126158/4ad4bc02-e445-4685-8895-2f46a8615b72)


6. Voici le résultat depuis ma machine physique :
![image1](https://github.com/Rymuu/ynov-resources/assets/96126158/544497b0-b335-42d5-9d04-b5a74992fb03)

## Worker

Je lance la commande suivante pour créer le Dockerfile de "Worker" afin de construire l'image :

`nano Dockerfile`

* Voici le contenu du ficher lisible en lançant la commande suivante :

`cat Dockerfile`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/1a46a85a-6252-4fc5-9bd1-cf82cd642345)


## Vote

Je lance la commande suivante pour créer le Dockerfile de "Vote" afin de construire l'image :

* Voici le contenu du ficher lisible en lançant la commande suivante :

`cat Dockerfile`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/66bddfe2-c37e-4c34-a350-06c6bc70c34c)


## Seed-data

Je lance la commande suivante pour créer le Dockerfile de "Seed-data" afin de construire l'image :


* Voici les commandes :
`nano Dockerfile`

* Voici le contenu du ficher lisible en lançant la commande suivante :

`cat Dockerfile`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/138083bd-5c79-46c1-baf0-2bdced37ba26)


## Result

Je lance la commande suivante pour créer le Dockerfile de "Result" afin de construire l'image :


* Voici les commandes :
`nano Dockerfile`

* Voici le contenu du ficher lisible en lançant la commande suivante :

`cat Dockerfile`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/d56ed1cc-4d5b-46a9-b07b-583549357e02)


## Création des images

* Création du docker compose qui s'occupe de lancer chaque Dockerfile précédemment crée pour créer les images.

* docker-compose-build.yaml:

* Contenu du docker compose :

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/3862270a-eb89-425f-8ecb-31f35cfb0f7b)

* Commandes pour lancer le docker compose :
  
`nano docker-compose-build.yaml`
`docker compose -f docker-compose-build.yaml build --parallel`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/cb32cfca-7aa0-418f-ad79-c2c60dd61c8e)


## Publication des images dans "registry"

* On publie chaque image construite dans "registry" :
1. Image "worker" :

`docker tag worker localhost:5000/worker`
`docker push localhost:5000/worker`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/dad0635a-93f1-410e-a4ca-6a2972419ec5)

2. Image "vote" :

`docker tag vote localhost:5000/vote`
`docker push localhost:5000/vote`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/9d3ccd9c-3bde-4194-8e6c-d839f7882585)

3. Image "seed-data" :

`docker tag seed-data localhost:5000/seed-data`
`docker push localhost:5000/seed-data`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/4c49c64e-8370-44a7-a88b-fc80a06d2b52)


4. Image "result" :

`docker tag result localhost:5000/result`
`docker push localhost:5000/result`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/4c8c194f-8532-45de-9dfd-0ccbda3b7676)


* Je vérifie si toutes les images ont bien été publiées dans le registre : 

`curl localhost:5000/v2/_catalog`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/2316f3c4-d366-4539-b105-63bd51f19d2b)

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/04d1fbc9-17c8-49b9-abec-3a3f31212db2)


## Publication sur Docker Hub

* Je me connecte à Docker Hub avec la commande suivante :

`docker login`

* On publie chaque image sur Docker Hub :

1. Image "worker" :

`docker tag worker rymuu/worker:v1`
`docker push rymuu/worker:v1`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/ad95d1a4-57e7-49cd-915a-8d0903c6ba25)

2. Image "vote" :

`docker tag vote rymuu/vote:v1`
`docker push rymuu/vote:v1`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/2d9eb6c9-138a-482d-bbb8-159bfaa7ce8e)

3. Image "seed-data" :

`docker tag seed-data rymuu/seed-data:v1`
`docker push rymuu/seed-data:v1`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/814c8cf6-bdd6-421c-b0ef-a8b612abd843)

4. Image "result" :

`docker tag result rymuu/result:v1`
`docker push rymuu/result:v1`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/a3f221e6-75bb-4602-afc4-1c55781bb670)

* Mes images sur Docker Hub :

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/7ff34135-d5f8-4558-8a33-e1ac82e88648)

* Je peux donc les pull en tapant `Docker pull` puis le nom de l'image.

## Création du Docker compose

* Création d'un docker compose.

* On lance le docker compose avec la commande suivante:

`docker compose up -d`

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/bb86714c-09c1-424f-ade1-3af436deb811)


## Résultat

* Port forwading dans la machine : 

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/d8d5cdb5-baf4-4590-a0d1-2864635b520e)


* Résultat : 

![image](https://github.com/Rymuu/ynov-resources/assets/96126158/3455d689-a442-46ca-aa20-bf4e2ef57e3b)

