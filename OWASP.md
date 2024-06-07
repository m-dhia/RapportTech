## Introduction
OWASP Dependency-Check est un outil de sécurité open-source qui analyse les dépendances d'une application afin d'identifier les composants logiciels présentant des vulnérabilités connues.

## Installation du plugin

### Gestionnaire de plugins Jenkins 
* Ouvrez le tableau de bord de Jenkins.
 * Accédez à "Gérer Jenkins" -> "Gérer les plugins".

### Installation du plugin OWASP Dependency-Check
 * Recherchez le plugin "OWASP Dependency-Check" et cochez la case à côté.
* Cliquez sur "Installer" et attendez la fin de l'installation.
![[OWASP-1.png]]
### Configuration de Dependency-Check
* Accédez à "Gérer Jenkins" -> "Configuration globale".
* Faites défiler jusqu'à la section "Installation de Dependency Check".
* Cliquez sur "Ajouter Dependency Check".

### Configuration de l'installation
 * Donnez un nom à l'installation (ex : Dependency-Check).
 * Cochez la case "Installer automatiquement".
 * Sélectionnez "Dependency-check" dans le menu déroulant "Ajouter un installateur".
 * Choisissez la version souhaitée.
 * (Facultatif) Configurez les options supplémentaires si nécessaire.
* Cliquez sur "Enregistrer".
![[OWASP-2.png]]
## Utilisation

### Jenkins
``` groovy 
stage('OWASP Dependency-Check Vulnerabilities') {
      steps {
        dependencyCheck additionalArguments: ''
        '  -
        o './' -
          s './' -
          f 'ALL'
          --prettyPrint ''
        ', odcInstallation: '
        OWASP Dependency - Check Vulnerabilities '

        dependencyCheckPublisher pattern: 'dependency-check-report.xml'
      }
    }
```

