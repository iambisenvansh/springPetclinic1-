
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
                withEnv(['JAVA_TOOL_OPTIONS=-Duser.timezone=Asia/Kolkata']) {
                    bat 'mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 test'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQube') {
                    bat 'mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 org.sonarsource.scanner.maven:sonar-maven-plugin:5.0.0.4389:sonar -Dsonar.projectKey=spring-petclinic'
                }
            }
        }

        stage('OWASP Dependency-Check') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'nvd-api-key',
                        variable: 'NVD_API_KEY'
                    )
                ]) {
                    bat '''
                        mvnw.cmd -B -Dmaven.repo.local=C:\\JenkinsData\\m2 org.owasp:dependency-check-maven:12.1.0:check -DnvdApiKeyEnvironmentVariable=NVD_API_KEY -DnvdMaxRetryCount=10 -DossIndexAnalyzerEnabled=false -DfailBuildOnCVSS=11
                    '''
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
    }
}
