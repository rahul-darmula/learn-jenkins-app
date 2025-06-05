pipeline {
    stages {
        agent {
            docker {
                image 'node:18-alpine'
                reuseNode true
            }
        }
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
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'Test stage.....'
                    test -f build/index.html
                    npm test 
                '''
            }
        }
    }
}