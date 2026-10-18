
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
    }

    post {
        success {
            echo 'CI Pipeline completed successfully!'
        }
        failure {
            echo 'CI Pipeline failed. Check the console output.'
        }
    }
}
