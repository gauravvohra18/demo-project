pipeline {
    agent {
        docker {
            image 'node:14'
        }
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/gauravvohra18/demo-project'
            }
        }
        stage('Build') {
            steps {
                sh 'echo Building application...'
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh 'echo Running tests...'
                sh 'npm test'
            }
        }
        stage('Docker Build & Run') {
            steps {
                script {
                    // Build the Docker image locally
                    def dockerImage = docker.build("demo-project:${env.BUILD_NUMBER}")
                    
                    // Run the Docker container locally
                    sh '''
                    docker stop demo-project || true
                    docker rm demo-project || true
                    docker run -d -p 8080:8080 --name demo-project demo-project:${env.BUILD_NUMBER}
                    '''
                }
            }
        }
    }
}
