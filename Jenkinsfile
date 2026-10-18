
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is ready'
            }
        }

        stage('Build') {
            steps {
                bat 'mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 -DskipTests package'
            }
        }

        stage('Unit Tests') {
            steps {
                bat 'mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 test'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 sonar:sonar -Dsonar.projectKey=spring-petclinic'
                }
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully!'
        }

        failure {
            echo 'CI Pipeline failed. Check the console output.'
        }

        always {
            echo 'Pipeline execution finished.'
        }
    }
}
