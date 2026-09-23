pipeline {

    agent any

    tools {
        maven 'maven'
    }

    environment {
        SONAR_PROJECT_KEY = 'java-app'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Ajit-Swami/sonar.git'
            }
        }

        stage('Build') {
            steps {
                sh '''
                    echo "Building Java application..."
                    mvn clean package
                '''
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('my-sonarqube') {
                    sh '''
                        echo "Running SonarQube analysis..."

                        mvn sonar:sonar \
                          -Dsonar.projectKey=${SONAR_PROJECT_KEY}
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

    post {

        success {
            echo 'Pipeline completed successfully!'
            echo 'SonarQube Quality Gate: PASSED'
        }

        failure {
            echo 'Pipeline failed. Check the Jenkins console output.'
        }

        always {
            echo 'Pipeline execution completed.'
        }
    }
}

