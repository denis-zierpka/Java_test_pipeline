pipeline {
    agent any

    tools {
        maven 'maven'
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
                        results: [[path: 'target/allure-results']]
                    )
                }
            }
        }

        stage('SonarQube') {
            steps {
                withSonarQubeEnv("SonarQube_server") {
                    script {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('Check') {
            steps {
                echo 'check'
            }
        }
    }
}