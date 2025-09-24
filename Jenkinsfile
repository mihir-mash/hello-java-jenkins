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
                sh 'mvn clean install'
            }
        }
        stage('SonarQube Analysis') {
	    steps {
        	withSonarQubeEnv('sonarqube') {
            		withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_LOGIN')]) {
                		sh 'mvn sonar:sonar -Dsonar.login=$SONAR_LOGIN'
            		}
            		waitForQualityGate(abortPipeline: true)
        	}
    	}
	}
    }
}
