pipeline {
    agent any
    environment {
        // Define your Docker Hub credentials ID here
        DOCKERHUB_CREDENTIALS = 'dockerhub'
        // Define your Docker image name and tag
        DOCKER_IMAGE = 'sahuhrithik/basic-banking:latest'
        NODE_ENV = 'production' 
    }
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
                sh 'npm install'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv {
                        sh "${scannerHome}/bin/sonar-scanner"
                    }
                }
            }
        }
        
        stage('Containerize (Docker)') {
            steps {
                // Build a Docker image for the Node.js app
                sh 'docker build -t sahuhrithik/basic-banking .'
                // Push the Docker image to a registry (if needed)
                // sh 'docker push your-docker-image-name'
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                script {
                    // Authenticate with Docker Hub using credentials
                    docker.withRegistry('https://registry.hub.docker.com', DOCKERHUB_CREDENTIALS) {
                        // Push the Docker image to Docker Hub
                        docker.image(DOCKER_IMAGE).push()
                    }
                }
            }
        }
        
        stage('Build and Run MongoDB Container') {
            steps {
                // Run the Docker command to start the MongoDB container
                sh 'docker run -d --name mongo -p 27018:27017 ' +
                   '-e MONGO_INITDB_ROOT_USERNAME=mongoadmin ' +
                   '-e MONGO_INITDB_ROOT_PASSWORD=App123Password ' +
                   '-e MONGO_INITDB_DATABASE=bankDB mongo'
            }
        }

        stage('Wait for MongoDB to Start') {
            steps {
                // Add a sleep step to give MongoDB some time to start (adjust as needed)
                sh 'sleep 30'
            }
        }

        stage('Connect to MongoDB and Grant Permissions') {
            steps {
                // Connect to MongoDB and grant permissions using mongosh
                sh 'docker run --link mongo:mongo --rm mongo mongosh ' +
                   '--host mongo -u mongoadmin -p App123Password ' +
                   '--authenticationDatabase admin bankDB ' +
                   '--eval "use admin; db.grantRolesToUser(\'mongoadmin\', [{ role: \'readWrite\', db: \'bankDB\' }]);"'
            }
        }

         stage('Deploy') {
            steps {
                script {
                    sh 'npm start'
                }
            }
        }
    }
}
