pipeline {
    agent any
    tools {
        maven 'M3'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/mihir-mash/hello-java-jenkins.git'
            }
        }
        stage('Build and Test') {
            steps {
                // This command compiles the code, runs tests, and packages it into a JAR
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                    sh 'mvn sonar:sonar -Dsonar.login=$SONAR_LOGIN'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                script {
                    def qg = waitForQualityGate()
                    if (qg.status != 'OK') {
                        error "Pipeline aborted due to Quality Gate failure: ${qg.status}"
                    }
                }
            }
        }
    }
}
