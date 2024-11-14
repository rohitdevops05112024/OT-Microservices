pipeline {
    agent any

    environment {
        GO_VERSION = '1.18' // Set the Go version if needed
        GOPATH = '/go' // Define Go workspace path
    }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from GitHub
                git branch: 'rohit_test', url: 'https://github.com/rohitdevops05112024/OT-Microservices.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                script {
                    // Install Go dependencies using `go mod`
                    sh 'go mod tidy'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    // Build the Go application
                    sh 'go build -o employee-app ./employee'
                }
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    // Run Go tests
                    sh 'go test -v ./employee'
                }
            }
        }

        stage('Build Docker Image') {
            when {
                branch 'rohit_test' // Only build Docker image on this branch
            }
            steps {
                script {
                    // Build the Docker image
                    sh 'docker build -t rohit/employee-app ./employee'
                }
            }
        }

        stage('Publish Docker Image') {
            when {
                branch 'rohit_test' // Only push Docker image to the registry on this branch
            }
            steps {
                script {
                    // Publish the Docker image to Docker Hub or your private registry
                    sh 'docker push rohit/employee-app'
                }
            }
        }

        stage('Clean Up') {
            steps {
                script {
                    // Remove the built binaries and Docker images to keep the workspace clean
                    sh 'rm -rf employee-app'
                    sh 'docker rmi rohit/employee-app'
                }
            }
        }
    }

    post {
        always {
            // Actions to be taken after the pipeline execution
            cleanWs() // Clean up the workspace
        }
        success {
            // Actions to take on success
            echo 'Pipeline completed successfully.'
        }
        failure {
            // Actions to take on failure
            echo 'Pipeline failed.'
        }
    }
}
