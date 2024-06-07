## Introduction
Docker est une plateforme qui vous permet de développer, déployer et exécuter des applications à l'intérieur de conteneurs.
## Installation 
### Sur Machine

#### Mettez à jour votre liste de paquets : 
   ```bash
sudo apt-get update
```
#### Installation de Docker :
   ```bash
sudo apt-get install docker.io
```
![[Docker-1.png]]
#### Vérification de l'installation:
   ```bash
docker --version
```
#### Démarrage du service Docker :
   ```bash
sudo systemctl start docker
```
#### Enable Docker Service on Boot :
   ```bash
sudo systemctl enable docker
```
#### Autoriser l'utilisation de Docker sans sudo :
   ```bash
sudo usermod -aG docker $USER
```

### Plugins Jenkins
![[Docker-Plugins.png]]
## Utilisation

### Jenkins
#### Build
``` groovy 
stage('Build Docker Image') {
      steps {
        script {
          // Build the Docker image
          docker.build("${IMAGE_NAME}:${IMAGE_TAG}")
        }
      }
    }
```
#### Push
``` groovy 
stage('Push Docker Image') {
      steps {
        script {
          // Push the Docker image to the registry
          docker.withRegistry('', DOCKER_PASS) {
            def dockerImage = docker.image("${IMAGE_NAME}:${IMAGE_TAG}")
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }
    }
```

## Obtenir un jeton d'accès Docker Hub 
Un jeton d'accès Docker Hub est une alternative plus sécurisée à votre mot de passe pour vous authentifier auprès de Docker Hub. Il vous permet de contrôler les actions qu'une application peut effectuer sur votre compte. Voici comment obtenir un jeton d'accès Docker Hub : 
1. **Connectez-vous à votre compte Docker Hub** : Rendez-vous sur https://hub.docker.com/login et connectez-vous à votre compte Docker Hub. 
2. **Accédez aux paramètres de votre compte** : Cliquez sur votre avatar dans le coin supérieur droit de la page et sélectionnez "Mon compte" dans le menu déroulant. 
   ![[Rapport technique DevSecOps/Screenshots/DockerHub-1.png]]
3. **Ouvrez l'onglet "Sécurité"** : Dans la page de votre compte, cliquez sur l'onglet "Sécurité". 
   ![[DockerHub-2.png]]
4. **Créer un jeton d'accès** : Cliquez sur le bouton "Nouveau jeton d'accès". 
5. **Donnez un nom à votre jeton** : Saisissez un nom descriptif qui indique l'utilisation prévue du jeton (par exemple, "jeton-jenkins"). 
   ![[DockerHub-3.png]]
6. **Générer le jeton** : Une fois que vous avez défini le nom et les permissions, cliquez sur le bouton "Générer". 
7. **Copier et conserver le jeton en sécurité** : Docker Hub affichera votre jeton d'accès nouvellement créé. **Important** : Vous ne pourrez pas récupérer ce jeton une fois que vous aurez fermé la fenêtre. Copiez soigneusement le jeton et stockez-le en lieu sûr, idéalement dans un gestionnaire de mots de passe.
![[DockerHub-4.png]]
## Ajouter des identifiants Docker Hub à Jenkins 
Pour configurer Jenkins afin d'utiliser vos identifiants Docker Hub pour la construction et le déploiement d'images Docker, suivez ces étapes : 
- Connectez-vous à votre serveur Jenkins. 
- Allez dans "Credentials" depuis le menu principal. 
- Cliquez sur "Global credentials (domain/configuration)" dans le panneau de gauche. 
- Dans la liste déroulante "Type", sélectionnez "Nom d'utilisateur avec mot de passe". 
- Entrez un identifiant descriptif (par exemple, "docker-hub-credentials"). 
- Saisissez votre nom d'utilisateur Docker Hub dans le champ "Nom d'utilisateur". 
- Saisissez votre jeton d'accès Docker Hub (pas votre mot de passe) dans le champ "Mot de passe". Les jetons d'accès Docker Hub peuvent être créés dans les paramètres de votre profil sous "Sécurité". 
![[DockerHub-Credentials.png]]
- Cliquez sur "Enregistrer".


## Références
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-ubuntu-20-04