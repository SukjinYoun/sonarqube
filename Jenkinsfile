pipeline {
    agent any

    tools {
        nodejs 'NodeJS'
    }

    environment {
        SONAR_PROJECT_KEY = 'sukjin_project'
        PATH = "${tool 'NodeJS'}/bin:${tool 'SonarScanner'}/bin:${env.PATH}"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], userRemoteConfigs: [[url: 'https://github.com/SukjinYoun/sonarqube.git']]])
            }
        }

       

        
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh """
                        sonar-scanner \
                        -Dsonar.projectKey=${SONAR_PROJECT_KEY} \
                        -Dsonar.sources=./nodejs \
                        -Dsonar.host.url=https://sonarqube.momoiot.co.kr \
                        -Dsonar.token=sqp_e0955c1b673f738f95c8b286c4545e63c7b2d203 \
                        -Dsonar.exclusions=node_modules/**,test/**
                        
                    """
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
