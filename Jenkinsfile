pipeline {
    agent any

    stages {
        stage('Build') {
            agent{
                docker{
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
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                echo 'Running tests...'
                test -f build/index.html
                npm test
                '''
            }
        }

                stage('E2E'){
            agent{
                docker{
                    image 'mcr.microsoft.com/playright:v1.39.0-jammy'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install -g serve
                    node_modules/.bin/serve -s build
                    npx playright test
                '''
            }
        }

        stage('Deploy'){
            steps{
                echo 'Deploying...'
            }
        }
    }
    post{
        always {
            junit 'test-results/junit.xml'
        }
    }
}
