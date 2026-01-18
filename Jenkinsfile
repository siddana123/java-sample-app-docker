pipeline {
    agent { label "slave-1" }

    environment {
        SONARQUBE_ENV = 'sonarqube-server'
        IMAGE_NAME = 'java-app'
    }

    stages {

        stage('Build') {
            steps {
                dir('java-app') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                dir('java-app') {
                    withSonarQubeEnv("${SONARQUBE_ENV}") {
                        sh '''
                        mvn sonar:sonar \
                        -Dsonar.projectKey=java-app \
                        -Dsonar.projectName=java-app
                        '''
                    }
                }
            }
        }

        stage('Docker Build') {
            steps {
                dir('java-app') {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

    }   // ✅ closes stages block

    post {
        success {
            echo 'Build, SonarQube analysis, and Docker image creation completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}


