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
                    npm ci # install dependencies
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
                    test -f build/index.html
                    npm test
                '''
            }
        }
        stage('E2E'){
            agent{
                docker{
                    image 'pull mcr.microsoft.com/playwright:v1.51.1-noble'
                    reuseNode true
                }
            }
            steps{
                sh '''
                    npm install -g serve
                    serve -s build
                    npm playwright test
                '''
            }
        }
    }

    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}
