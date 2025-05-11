pipeline {
    agent any

    environment {
        // Force Docker CLI to use the unix socket instead of HTTPS
        DOCKER_HOST = 'unix:///var/run/docker.sock'
    }

    stages {
        stage('Build') {
               agent {
        docker {
            image 'node:18-alpine'
            args '-v /var/run/docker.sock:/var/run/docker.sock -u root'
        }
    }
            steps {
                sh '''
                    ls -la
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }

        stage('Test') {
            agent any
            steps {
                sh '''
                    test -f build/index.html
                    npm test
                '''
            }
        }
    }
}