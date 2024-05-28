pipeline {
    agent any
    tools {
        jdk 'JDK 21'
        maven 'Maven 3.9.7'
    }
    stages {
        stage('Build Maven') {
            steps {
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/hamzajaa/devops-integration']])
                script {
                    if (isUnix()) {
                        sh 'mvn clean install'
                    } else {
                        bat 'mvn clean install'
                    }
                }
            }
        }
        stage('Build Docker image') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'docker build -t hamzajaa/devops-integration .'
                    } else {
                        bat 'docker build -t hamzajaa/devops-integration .'
                    }
                }
            }
        }
        stage('Push Image To DockerHub'){
            steps {
                script {
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                         if (isUnix()) {
                            sh 'docker login -u hamzajaa -p %dockerhubpwd%'
                        } else {
                            bat 'docker login -u hamzajaa -p %dockerhubpwd%'
                        }
                    }
                    if(isUnix()) {
                        sh 'docker push hamzajaa/devops-integration'
                    } else {
                        bat 'docker push hamzajaa/devops-integration'
                    }
                }
            }
        }
    }
}
