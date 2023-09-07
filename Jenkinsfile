pipeline {
    agent any
    // environment {
    //     DOCKER_REGISTRY_CREDENTIALS = credentials('DockerHub')
    //     DOCKER_IMAGE_NAME = 'basic-banking'
    //     DOCKERFILE_PATH = 'Dockerfile'
    // }
    tools {
        // Install the Maven version configured as "M3" and add it to the path.
        maven "M3"
    }
    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
            git branch:'dev',
                url:'https://github.com/SahuHrithik/basic-banking-system.git'
            }            
        }

        stage('Build and Test') {
            steps {
                // Install Node.js and npm (if not already done)
                // Use Maven to build and test the Node.js application
                sh 'mvn clean install'
            }
        }

        stage('Containerize (Docker)') {
            steps {
                // Build a Docker image for the Node.js app
                sh 'sudo docker build -t basic-banking .'
                // Push the Docker image to a registry (if needed)
                // sh 'docker push your-docker-image-name'
            }
        }
 
        // stage('Static Code Analysis') {
        //     steps {
        //         // Run SonarQube analysis
        //         // Assumes SonarQube is configured in Jenkins
        //         withSonarQubeEnv('Your_SonarQube_Server') {
        //             sh 'mvn sonar:sonar'
        //         }
        //     }
        // }

        // stage('Build Docker Image') {
        //     steps {
        //         script {
        //                 def customImage = docker.build(DOCKER_IMAGE_NAME, "-f ${DOCKERFILE_PATH} .")
        //         }
        //     }
        // }
        // stage('Push Docker Image') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://registry.example.com', DOCKER_REGISTRY_CREDENTIALS) {
        //                 customImage.push()
        //             }
        //         }
        //     }
        // }
    }
}

 

        // stage('Deploy (Kubernetes)') {
        //     steps {
        //         // Deploy the Docker container to Kubernetes
        //         sh 'kubectl apply -f kubernetes-deployment.yaml'
        //     }
        // }

