pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp:${env.BUILD_NUMBER}"
        DOCKER_REGISTRY = "mydockerhubusername"
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the repository, specifying the 'main' branch
                checkout scm: [
                    $class: 'GitSCM', 
                    branches: [[name: '*/main']], // Make sure 'main' is specified
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    userRemoteConfigs: [[url: 'https://github.com/mohamedrebaii/spring-petclinic']]
                ]
            }
        }
        
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        
    
    post {
        success {
            echo "Pipeline completed successfully!"
        }
        failure {
            echo "Pipeline failed. Check the logs."
        }
    }
}

