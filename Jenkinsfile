pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "myapp:${env.BUILD_NUMBER}"
        DOCKER_REGISTRY = "mydockerhubusername"
    }

    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/spring-projects/spring-petclinic'
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
        
        stage('Code Coverage') {
            steps {
                sh 'mvn cobertura:cobertura'
            }
        }
        
        stage('Docker Build') {
            steps {
                script {
                    sh 'docker build -t $DOCKER_IMAGE .'
                }
            }
        }
        
        stage('Push to DockerHub') {
            steps {
                withDockerRegistry([credentialsId: 'dockerhub-credentials']) {
                    sh 'docker tag $DOCKER_IMAGE $DOCKER_REGISTRY/$DOCKER_IMAGE'
                    sh 'docker push $DOCKER_REGISTRY/$DOCKER_IMAGE'
                }
            }
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
