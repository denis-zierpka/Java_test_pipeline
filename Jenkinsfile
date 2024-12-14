pipeline {
    agent any

    tools {
        maven 'maven-3.8.1'
        allure 'allure'  
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    allure includeProperties:
                     false,
                     jdk: '',
                     results: [[path: 'target/allure-results']]
                }
            }
        }

        stage('Static Analysis') {
            steps {
                withSonarQubeEnv("SonarQube_server") {
                    script {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
                echo 'Static Analysis Completed'
            }
        }
        stage('Deploy app') {
            steps {
                script {
                    sh 'docker --version' 
                    sh 'docker build -f app/Dockerfile -t server:latest app'
                    sh 'docker-compose -f app/app-docker-compose.yml up -d'
                }
            }
        }
    }


    post {
        success {
            echo 'Success!'
        }
        failure {
            echo 'Failure!'
        }
    }
}