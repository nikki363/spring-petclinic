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
                    sh 'cp -r target /usr/share/jenkins/ref/spring-petclinic'
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
        stage('Run ansible playbook') {
            steps {
                dir('/usr/share/jenkins/ref') {
                    ansiblePlaybook becomeUser: 'niki', inventory: '/usr/share/jenkins/ref/inventory.ini', playbook: '/usr/share/jenkins/ref/deploy-petclinic.yml'
                }
            }
        }
    }
}

