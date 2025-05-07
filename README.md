# 🧠 Projet Fil Rouge - Application Django avec Analyse de Qualité (SonarQube + Jenkins)

Ce projet a été réalisé dans le cadre du programme **Orange Digital Center**. Il s'agit d'une application web développée avec **Python/Django**, conteneurisée avec **Docker**, et intégrée à un pipeline CI/CD pour analyser la qualité du code avec **SonarQube** et **Jenkins**.

---

## 📌 Objectifs

- Développer une application backend robuste avec Django
- Conteneuriser l’ensemble du projet pour faciliter le déploiement
- Mettre en place un système d’analyse automatique de la qualité du code
- Intégrer un pipeline d’intégration continue avec Jenkins

---

## 🛠️ Technologies utilisées

- Python 3 / Django
- Docker / Docker Compose
- PostgreSQL
- SonarQube
- Jenkins
- Git / GitHub

---

## ⚙️ Architecture du projet

```txt
├── app/                    # Code source Django
├── docker-compose.yml      # Configuration des conteneurs
├── Jenkinsfile             # Pipeline Jenkins
├── sonar-project.properties # Configuration SonarQube
└── README.md               # Présentation du projet
