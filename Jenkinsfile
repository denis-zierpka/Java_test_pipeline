pipeline {
    agent any

    tools {
        maven 'maven-3.8.1'
        allure 'allure'  
    }

    stages {
        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Test and allure') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    allure (
                        results: [[path: 'target/allure-results']],
                        includeProperties: false
                    )
                }
            }
        }

        // stage('SonarQube') {
        //     steps {
        //         withSonarQubeEnv("SonarQube_server") {
        //             script {
        //                 sh 'mvn clean package sonar:sonar'
        //             }
        //         }
        //     }
        // }
        // stage('Deploy app') {
        //     steps {
        //         script {
        //             sh 'docker --version' 
        //             sh 'docker build -f app/Dockerfile -t server:latest app'
        //             sh 'docker-compose -f app/app-docker-compose.yml up -d'
        //         }
        //     }
        // }
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