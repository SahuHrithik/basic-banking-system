pipeline {
    agent any
    environment {
        // Define your Docker Hub credentials ID here
        DOCKERHUB_CREDENTIALS = 'dockerhub'
        // Define your Docker image name and tag
        DOCKER_IMAGE = 'sahuhrithik/basic-banking:latest'
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
                sh 'mvn clean install'
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
        
        stage('Deploy MongoDB Container') {
            steps {
                script {
                    // Run the MongoDB container with the provided environment variables
                    def mongoContainer = docker.image('mongo').run(
                        '-d --name mongo -p 27017:27017' +
                        ' -e MONGO_INITDB_ROOT_USERNAME=mongoadmin' +
                        ' -e MONGO_INITDB_ROOT_PASSWORD=App123Password' +
                        ' -e MONGO_INITDB_DATABASE=bankDB'
                    )
                    
                    // Wait for the container to be up and running (adjust timeout as needed)
                    timeout(time: 5, unit: 'MINUTES') {
                        waitUntil {
                            def logs = mongoContainer.logs()
                            return logs.contains('waiting for connections')
                        }
                    }
                }
            }
        }
        
        stage('Grant MongoDB User Roles') {
            steps {
                script {
                    // Run commands inside the MongoDB container to grant roles
                    def mongoShellCmd = """
                        docker run --rm --link mongo:mongo mongo \
                        mongosh --host mongo -u mongoadmin -p App123Password --authenticationDatabase admin bankDB <<EOF
                        use admin
                        db.grantRolesToUser('mongoadmin', [{ role: 'readWrite', db: 'bankDB' }])
                        exit
                        EOF
                    """
                    sh(script: mongoShellCmd, returnStatus: true)
                }
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
