pipeline {
    agent any

    environment {
        PATH = "/opt/sonar-scanner/bin:$PATH"
        DOCKER_USER = 'mouhamed2555'
        BACKEND_IMAGE = "${DOCKER_USER}/projetfilrouge_backend"
        FRONTEND_IMAGE = "${DOCKER_USER}/projetfilrouge_frontend"
        MIGRATE_IMAGE = "${DOCKER_USER}/projetfilrouge_migrate"
        SONAR_HOST_URL = ' https://316e-41-214-71-39.ngrok-free.app/github-webhook'
'
        SONAR_BACK_TOKEN = credentials('sonar-scan')
        SONAR_FRONT_TOKEN = credentials('sonar-scan')
    }

    stages {
        stage('Cloner le dépôt') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/zouboss/projet_fil_rouge.git'
            }
        }

       /* stage('Tests') {
            steps {
                sh '''
                    cd Backend
                    pip install -r requirements.txt
                    pytest
                '''
                sh '''
                    cd Frontend
                    npm install
                    npm test
                '''
            }
        }
        */
        stage('Analyse SonarQube Backend') {
            steps {
                sh '''
                   sonar-scanner \
                         -Dsonar.projectKey=backend \
                         -Dsonar.sources= Backend \
                         -Dsonar.host.url=http://localhost:9000 \
                         -Dsonar.token=sqp_8bfd8bad6f38ab4299fd9464931f5f5b64082329
                '''
            }
        }

        stage('Analyse SonarQube Frontend') {
            steps {
                dir('Frontend') {
                    sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=frontend \
                            -Dsonar.sources=src \
                            -Dsonar.exclusions=**/node_modules/**,**/dist/**,**/build/** \
                            -Dsonar.host.url=$SONAR_HOST_URL \
                            -Dsonar.login=$SONAR_FRONT_TOKEN \
                            -Dsonar.javascript.lcov.reportPaths=coverage/lcov.info
                    '''
                }
            }
        }

        stage('Build des images') {
            steps {
                sh 'docker build -t $BACKEND_IMAGE:latest ./Backend/odc'
                sh 'docker build -t $FRONTEND_IMAGE:latest ./Frontend'
                sh 'docker build -t $MIGRATE_IMAGE:latest ./Backend/odc'
            }
        }

        stage('Push des images sur Docker Hub') {
            steps {
                withDockerRegistry([credentialsId: 'dimanche', url: '']) {
                    sh 'docker push $BACKEND_IMAGE:latest'
                    sh 'docker push $FRONTEND_IMAGE:latest'
                    sh 'docker push $MIGRATE_IMAGE:latest'
                }
            }
        }

        stage('Déploiement local avec Docker Compose') {
            steps {
                sh '''
                    docker-compose down || true
                    docker-compose pull
                    docker-compose up -d --build
                '''
            }
        }
    }

    post {
        success {
            mail to: 'alassanebenzecoly@gmail.com',
                 subject: "✅ Déploiement local réussi",
                 body: "L'application a été déployée localement avec succès."
        }
        failure {
            mail to: 'alassanebenzecoly@gmail.com',
                 subject: "❌ Échec du pipeline Jenkins",
                 body: "Une erreur s’est produite, merci de vérifier Jenkins."
        }
    }
}
