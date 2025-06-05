pipeline {
    agent {
        docker {
            image 'node:18-alpine'
            reuseNode true
        }
    }
    stages {
        stage('Build') {
            steps {
                sh '''
                    echo 'Build stage......'
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        stage('Test') {
            steps {
                sh '''
                    echo 'Test stage.....'
                    if test -f /var/jenkins_home/workspace/jenkins-pipeline-github-integration/build/index.html; then
                        echo 'File exists'
                    npm test 
                '''
            }
        }
    }
}