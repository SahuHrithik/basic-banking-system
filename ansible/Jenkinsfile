pipeline {
    agent any
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }
    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
            git branch:'main',
                url:'https://github.com/Techwidot/basic-banking-system.git'

 

            }            
        }

        stage('Build and Test') {
            steps {
                // Install Node.js and npm (if not already done)
                // Use Maven to build and test the Node.js application
                sh 'mvn clean install'
            }
        }

 

        stage('Static Code Analysis') {
            steps {
                // Run SonarQube analysis
                // Assumes SonarQube is configured in Jenkins
                withSonarQubeEnv('Your_SonarQube_Server') {
                    sh 'mvn sonar:sonar'
                }
            }
        }

 

        stage('Containerize (Docker)') {
            steps {
                // Build a Docker image for the Node.js app
                sh 'docker build -t your-docker-image-name .'
                // Push the Docker image to a registry (if needed)
                // sh 'docker push your-docker-image-name'
            }
        }

 

        stage('Deploy (Kubernetes)') {
            steps {
                // Deploy the Docker container to Kubernetes
                sh 'kubectl apply -f kubernetes-deployment.yaml'
            }
        }

 

 

    }
}
