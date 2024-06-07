## Introduction
Jenkins est un serveur d'intégration continue open-source qui automatise le processus de construction, de test et de déploiement de logiciels. 
## Installation 
### Ajoutez la clé de dépôt Jenkins :
 ```bash 
sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
 ``` 
### Ajoutez le dépôt Jenkins à votre liste de sources :
```bash
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
``` 
### Mettez à jour votre liste de paquets :
 ```bash
sudo apt-get update 
``` 
### Installez Jenkins :
```bash
sudo apt-get install jenkins
``` 
### Start Jenkins
```bash
sudo systenctl start jenkins
``` 
### Enable Jenkins
```bash
sudo systenctl enable jenkins
``` 
### Status Jenkins
```bash
sudo systenctl status jenkins
``` 
![[Jenkins-status.jpg]]
### Accès à l'interface web de Jenkins :
-  Une fois Jenkins installé, accédez à `http://your_server_ip_or_domain:8080` dans votre navigateur web.
### Trouver le mot de passe initial de Jenkins :
- Trouvez le mot de passe initial de Jenkins en utilisant la commande suivante :
   ```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```
![[Jenkins-pass.jpg]]
![[Jenkins-pass-2.jpg]]
### Installation des plugins suggérés :
- Après avoir déverrouillé Jenkins, vous serez invité à installer les plugins suggérés. Sélectionnez les plugins à installer automatiquement ou choisissez de les installer manuellement.
![[Jenkins-plugin.jpg]]
### Création d'un compte administrateur :
- Créez un compte administrateur pour Jenkins.
  ![[Jenkins-admin.jpg]]

## Utilisation ( Création d'un projet de pipeline Jenkins ) 
### Créer un nouvel élément : 
- Connectez-vous au tableau de bord de Jenkins. 
- Cliquez sur "Nouveau projet" dans la barre de navigation de gauche.
### Choisir un projet Pipeline : 
- Sélectionnez "Pipeline" sous "Type d'élément" et cliquez sur "OK". 
### Configurer la définition du pipeline : 
- Sous "Pipeline", choisissez "Script Pipeline depuis SCM". 
  ![[Pipeline SCM.jpg]]
### Définir l'emplacement SCM : 
- Dans la section "SCM", sélectionnez "Git" comme système de contrôle de version. 
- Entrez l'URL de votre dépôt Git dans le champ "URL du dépôt". 
- Spécifiez la branche contenant votre Jenkinsfile dans le champ "Branche" (par exemple, main, master).
- Dans le champ "Chemin du script", indiquez le chemin relatif de votre Jenkinsfile dans le dépôt (par exemple, Jenkinsfile). 
  ![[Pipeline-Git-1.jpg]]
  ![[Pipeline-Git-2.jpg]]
### Enregistrer et construire :
- Cliquez sur "Enregistrer" pour créer le projet de pipeline.
### Déclenchement optionnel de la construction initiale : 
- Après l'enregistrement, vous pouvez déclencher manuellement la première construction en cliquant sur le bouton "Construire maintenant".

## Références
https://www.jenkins.io/doc/book/installing/