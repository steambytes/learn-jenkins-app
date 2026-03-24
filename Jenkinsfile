pipeline {
    agent any

    stages {
        /*stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh'''
                    ls -la
                    node --version
                    npm --version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }
        */
        stage('Test') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh'''
                    echo "-----Test stage-----"
                    #FILE=./build/index.html
                    #test -f "$FILE" && echo "$FILE exists."
                    npm test
                '''
            }
        }

        stage('E2E') {
            agent {
                docker {
                    image 'mcp/playwright:latest'
                    reuseNode true
                    args "--entrypoint=''"
                }
            }
            steps {
                sh'''
                    npm install serve
                    node_modules/.bin/serve -s build &
                    sleep 10
                    npx playwright
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