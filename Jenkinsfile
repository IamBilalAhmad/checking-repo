pipeline {
    agent any

    environment {
        SONARQUBE_TOKEN = credentials('sonarqube-jenkins-token')
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/IamBilalAhmad/checking-repo.git',
                    credentialsId: 'jenkins-github-access'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('MySonarQube') {
                    sh """
                      sonar-scanner \
                      -Dsonar.projectKey=checking-repo \
                      -Dsonar.sources=. \
                      -Dsonar.host.url=http://localhost:9000 \
                      -Dsonar.login=$SONARQUBE_TOKEN
                    """
                }
            }
        }

        stage('Docker Build') {
            steps {
                script {
                    docker.build("checking-repo")
                }
            }
        }
    }
}

