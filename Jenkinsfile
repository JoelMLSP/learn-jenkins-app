pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = "9d2f94cd-bd25-4024-ab40-7b42a12d0a0b"
        NETLIFY_AUTH_TOKEN = credentials('netlify-token')
    }

    stages {
        stage('build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
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
                    test -f ./build/index.html
                    npm test
                '''

            }
        }

        stage('deploy') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                    npm install netlify-cli
                    node_modules/.bin/netlify --version
                    echo "deploying to production Site ID: $NETLIFY_SITE_ID"
                    node_modules/.bin/netlify status
                '''
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}
