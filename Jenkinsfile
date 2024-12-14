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

        stage('SonarQube') {
            steps {
                withSonarQubeEnv("SonarQube_server") {
                    script {
                        sh 'mvn clean package sonar:sonar'
                    }
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