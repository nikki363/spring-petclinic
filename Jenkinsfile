pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                sh 'rm -rf spring-petclinic/'
                sh 'git clone https://github.com/spring-projects/spring-petclinic.git'
            }
        }
        stage('Build') {
            steps {
                dir('spring-petclinic') {
                    sh './mvnw clean package -DskipTests=true'
                }
            }
        }
        stage('Test') {
            steps {
                dir('spring-petclinic') {
                    sh './mvnw test'
                }
            }
        }
        stage('SonarQube') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    dir('spring-petclinic') {
                        sh './mvnw clean verify sonar:sonar -Dsonar.projectKey=CI-Spring-new'
                    }
                }
            }
        }
        stage('Build petclinic.jar') {
            steps {
                dir('spring-petclinic/target') {
                    sh 'java -Dserver.port=8888 -jar spring-petclinic-*.jar'
                }
            }
        }
    }
}

