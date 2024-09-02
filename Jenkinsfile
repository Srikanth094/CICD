pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SONAR_URL = "http://18.215.143.242:9000"
        SCANNER_HOME=tool 'sonar-scanner'
        SONAR_AUTH_TOKEN = "Sonar-token"
    }

    stages {
        /*stage('Checkout scm') {
            steps {
               // git branch: 'main', url: 'https://github.com/Srikanth094/CICD.git'
            }

        }*/

        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('sonar-scanner') {
                    script {
                    sh '''
                     $SCANNER_HOME/bin/sonar-scanner \
                     -Dsonar.projectKey=nodejs \
                     -Dsonar.projectName=nodejs \
                     -Dsonar.sources=. \
                     -Dsonar.host.url=${SONAR_URL}  \
                     -Dsonar.login=${SONAR_AUTH_TOKEN}
                     '''
                }
                }
            }
        }

        stage('Quality Gate') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }

        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
    }
}