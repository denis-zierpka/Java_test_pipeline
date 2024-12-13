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
                withSonarQubeEnv("SonarServer1") {
                    script {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
                echo 'Static Analysis Completed'
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