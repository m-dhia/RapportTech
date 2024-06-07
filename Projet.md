## Pipeline CI/CD avec Analyse de Sécurité 

[Prérequis](Prérequis)

Ce diagramme Markdown représente un pipeline CI/CD intégrant l'analyse de sécurité :

graph LR
A Push code vers [Github](Github) --> B{Déclencher Tâche Jenkins}
B --> C{Build ([Jenkins](Jenkins))}
C --> D{Scan [OWASP](OWASP) }
D --> E{Analyse [SonarQube](SonarQube)}
E --> F{[Dockerisation](Docker) de l'application}
F --> G {Analyse [Trivy](Trivy)}
G --> H{Push vers [Docker Hub](Docker)}
