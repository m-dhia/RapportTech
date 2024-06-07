## Introduction
Trivy est un scanner de sécurité open-source conçu pour analyser les images Docker, les systèmes de fichiers et les référentiels Git afin de détecter les vulnérabilités et les configurations incorrectes. 

## Installation
### Téléchargez le paquet Trivy : 
``` bash 
wget https://github.com/aquasecurity/trivy/releases/download/v0.18.3/trivy_0.18.3_Linux-64bit.deb
```
![[Rapport technique DevSecOps/Screenshots/Trivy-1.png]]
### Installez le paquet téléchargé :
``` bash 
sudo dpkg -i trivy_0.18.3_Linux-64bit.deb
```
![[Trivy-2.png]]

## Utilisation
### Jenkins

``` groovy 
stage('Trivy Scan') {
  steps {
    script {
      def timestamp = sh(script: "date +'%S-%M-%H-%d-%m-%Y'", returnStdout: true).trim()
      sh "touch TrivyLogs-${timestamp}.txt"
      sh "trivy ${IMAGE_NAME}:${IMAGE_TAG} > TrivyLogs-${timestamp}.txt"
    }
  }
}
```

## Références
https://github.com/aquasecurity/trivy