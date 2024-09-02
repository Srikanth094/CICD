pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }
    environment {
        SONAR_URL = "http://18.215.143.242:9000"
        SCANNER_HOME=tool 'sonar-scanner'
        SONAR_AUTH_TOKEN = 'Sonar-token'
    }

    stages {
        /*stage('Checkout scm') {
            steps {
               // git branch: 'main', url: 'https://github.com/Srikanth094/CICD.git'
            }

        }*/

        stage('Sonarqube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '$SCANNER_HOME/bin/sonar-scanner -Dsonar.login=${SONAR_AUTH_TOKEN} -Dsonar.host.url=${SONAR_URL} -Dsonar.projectName=nodejs \
                    -Dsonar.projectKey=nodejs'
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