pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'da60fd6c-45a5-4f5b-b91b-eaa0bee6c0b0'
        NETLIFY_AUTH_TOKEN = credentials('netlify-token') 
    }

    stages {
        
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    echo 'adding a line to check poll'
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
        /*
        stage('E2E_test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.52.0-noble'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install serve
                    node_modules/.bin/serve -s build
                    sleep 10
                    npx playwright test
                '''
            }
        }
        */
        stage(Deploy) {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli@20.1.1
                    node_modules/.bin/netlify --version
                    node_modules/.bin/netlify status
                    node_modules/.bin/netlify deploy --dir=build --prod
                '''
            }
        }
    }

    post {
        always {
            junit 'test-results1/junit.xml'
        }
    }
}