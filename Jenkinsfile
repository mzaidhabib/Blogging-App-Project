pipeline {
    agent any
    tools {
        maven 'maven-3'
        jdk 'jdk-17'
    }
    environment {
        SONAR_HOME= tool 'sonar-scanner'
    }
    stages {
        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                    sh '''
                        ${SONAR_HOME}/bin/sonar-scanner \
                        -Dsonar.projectKey=Blogging-App-Project \
                        -Dsonar.sources=. \
                        -Dsonar.java.binaries=target
                    '''
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}