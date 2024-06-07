## Introduction
SonarQube est une plateforme open-source de qualité du code qui permet aux développeurs de suivre et d'améliorer la qualité de leur code source.
## Installation

### Install Postgresql 15

```
sudo apt update
sudo apt upgrade

sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'

wget -qO- https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo tee /etc/apt/trusted.gpg.d/pgdg.asc &>/dev/null

sudo apt update
sudo apt-get -y install postgresql postgresql-contrib
sudo systemctl enable postgresql
```

### Create Database for Sonarqube

```
sudo passwd postgres
su - postgres

createuser sonar
psql 
ALTER USER sonar WITH ENCRYPTED password 'sonar';
CREATE DATABASE sonarqube OWNER sonar;
grant all privileges on DATABASE sonarqube to sonar;
\q

exit
```

### Increase Limits

```
sudo vim /etc/security/limits.conf
```
Paste the below values at the bottom of the file
```
sonarqube   -   nofile   65536
sonarqube   -   nproc    4096
```
```
sudo vim /etc/sysctl.conf
```
Paste the below values at the bottom of the file
```
vm.max_map_count = 262144
```
Reboot to set the new limits
```
sudo reboot
```

### Install Sonarqube 
```
sudo wget https://binaries.sonarsource.com/Distribution/sonarqube/sonarqube-9.9.0.65466.zip
sudo apt install unzip
sudo unzip sonarqube-9.9.0.65466.zip -d /opt
sudo mv /opt/sonarqube-9.9.0.65466 /opt/sonarqube
sudo groupadd sonar
sudo useradd -c "user to run SonarQube" -d /opt/sonarqube -g sonar sonar
sudo chown sonar:sonar /opt/sonarqube -R
```
Mettez à jour les propriétés de SonarQube avec les identifiants de la base de données
```
sudo vim /opt/sonarqube/conf/sonar.properties
```
Trouvez et remplacez les valeurs ci-dessous, vous devrez peut-être ajouter l'URL JDBC de SonarQube
```
sonar.jdbc.username=sonar
sonar.jdbc.password=sonar
sonar.jdbc.url=jdbc:postgresql://localhost:5432/sonarqube
```
Créez un service pour SonarQube
```
sudo vim /etc/systemd/system/sonar.service
```
Collez ce qui suit dans le fichier
```
[Unit]
Description=SonarQube service
After=syslog.target network.target

[Service]
Type=forking

ExecStart=/opt/sonarqube/bin/linux-x86-64/sonar.sh start
ExecStop=/opt/sonarqube/bin/linux-x86-64/sonar.sh stop

User=sonar
Group=sonar
Restart=always

LimitNOFILE=65536
LimitNPROC=4096

[Install]
WantedBy=multi-user.target
```
Démarrez SonarQube et activez le service
```
sudo systemctl start sonar
sudo systemctl enable sonar
sudo systemctl status sonar
sudo tail -f /opt/sonarqube/logs/sonar.log
```
### Accédez à l'interface utilisateur de SonarQube
```
http://your_server_ip_or_domain:9000
```

## Utilisation
### Configuration du plugin SonarQube Scanner
1. **Installation du plugin** 
* Ouvrez le tableau de bord de Jenkins. 
* Accédez à "Gérer Jenkins" -> "Gérer les plugins".
* Recherchez le plugin "SonarQube Scanner" et cochez la case à côté. 
* Cliquez sur "Installer" et attendez la fin de l'installation.
3. **Configuration de SonarQube** 
* Accédez à "Gérer Jenkins" -> "Configurer le système". 
* Faites défiler jusqu'à la section "SonarQube Servers" et cliquez sur "Ajouter SonarQube". 
* Saisissez les informations suivantes : 
* **Nom** : Donnez un nom unique à votre instance SonarQube.
* **URL du serveur** : L'URL de votre instance SonarQube.
* **Jeton d'authentification** : Générez un jeton d'accès dans SonarQube (Utilisateur -> Mon compte -> Sécurité) et collez-le ici. * Cliquez sur "Enregistrer".
4. **Configuration du scanner** 
* Créez un nouveau job Jenkins ou modifiez un job existant. 
* Dans la section "Ajouter une étape de build", recherchez et sélectionnez "Analyser avec SonarQube Scanner".
* Configurez les options du scanner, telles que :
* **Projet SonarQube** : Le projet SonarQube dans lequel vous souhaitez analyser votre code. 
* **Langages** : Les langages de programmation utilisés dans votre projet. * (Facultatif) Autres options avancées selon vos besoins. 
### Accès à SonarQube
   * Ouvrez votre navigateur web et accédez à l'URL de votre instance SonarQube.
   * Connectez-vous à SonarQube en utilisant vos identifiants.
### Création d'un projet
  1. **Création du projet** 
	 * Sélectionnez "Manually".
	![[Rapport technique DevSecOps/Screenshots/sonarqube-project-1.png]]

   2. **Creation du projet** 
	   * Saisissez les informations suivantes : 
	   * **Clé du projet** : Un identifiant unique pour votre projet. 
	   * **Nom du projet** : Un nom descriptif pour votre projet. 
		![[Rapport technique DevSecOps/Screenshots/sonarqube-project-2.png]]
3. **Configuration du projet** 
	![[sonarqube-project-3.png]]
4. **Choisir l'outil à intégrer** 
	![[sonarqube-project-4.png]]
5. **Choisissez vos options** 
	![[sonarqube-project-5.png]]
	![[sonarqube-project-6.png]]
	![[sonarqube-project-7.png]]
	![[sonarqube-project-8.png]]
	![[sonarqube-project-9.png]]
	![[sonarqube-project-10.png]]

### Jenkins
``` groovy 
stage('SonarQube Analysis') {
      steps {
        script {
          def scannerHome = tool 'SonarScanner';
          withSonarQubeEnv() {
            sh "${scannerHome}/bin/sonar-scanner"
          }
        }
      }
    }
```

