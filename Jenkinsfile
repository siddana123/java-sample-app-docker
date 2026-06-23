pipeline {
    agent { label "slave-1" } // ✅ closes agent block

    environment {
        SONARQUBE_ENV = 'sonarqube-server'
        IMAGE_NAME = 'java-app'
    } // ✅ closes environment block 

    stages {

        stage('Build') {
            steps {
                dir('java-app') {
                    sh 'mvn clean package -DskipTests'
                }
            }
        } // ✅ closes Build stage: Deployment

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
        } // ✅ closes SonarQube Analysis stage: scanning static code anaLYSIS

        stage('Docker Build') {
            steps {
                dir('java-app') {
                    sh 'docker build -t ${IMAGE_NAME}:latest .'
                }
            }
        }

    }   // ✅ closes stages block
    // ✅ closes pipeline block

    post {
        success {
            echo 'Build, SonarQube analysis, and Docker image creation completed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
    // ✅ closes post block
}


