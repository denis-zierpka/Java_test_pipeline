pipeline {
    agent any

    tools {
        maven 'maven-3.8.1'
        allure 'allure'  
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
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
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -f app/Dockerfile -t client:latest app'
                }
            }
        }
        // stage('Deploy') {
        //     steps {
        //         sh 'docker compose stop'
        //         sh 'docker compose down'
        //         sh 'docker compose up -d'
        //     }
        // }
    }


    post {
        success {
            echo 'Сборка прошла успешно!'
        }
        failure {
            echo 'Сборка завершилась с ошибкой.'
        }
    }
}