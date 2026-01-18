pipeline {
    agent /sonarqube-webhook/

    stages {
       stage('git checkout') {
                                  steps {
                                            // Get some code from a GitHub repository
                                            git 'https://github.com/siddana123/java-sample-app-docker.git'
                                        }
                                  }
    
    
    
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube-server') {
                    sh '''
                    mvn sonar:sonar \
                    -Dsonar.projectKey=java-app \
                    -Dsonar.projectName=java-app
                    '''
                }
            }
        }

        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
