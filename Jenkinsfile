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
        stage('Tests') {
            parallel {
                stage('Unit tests') {
                    agent {
                        docker {
                            image 'node:18-alpine'
                            reuseNode true
                        }
                    }
                    steps {
                        sh'''
                            echo "-----Test stage-----"
                            FILE=./test-results/junit.xml
                            test -f "$FILE" && echo "$FILE exists."
                            ls -al test-results/
                            npm test
                            ls -al test-results/
                        '''
                    }
                }

                stage('E2E') {
                    agent {
                        docker {
                            image 'mcr.microsoft.com/playwright:v1.58.2-jammy'
                            reuseNode true
                        }
                    }
                    steps {
                        sh'''
                            ls -al test-results/
                            npm install serve
                            node_modules/.bin/serve -s build &
                            sleep 10
                            npx playwright test --reporter=html
                            ls -al test-results/
                        '''
                    }
                }
            }
        }
    }
    post {
        always {
            junit 'test-results/junit.xml'
        }
    }
}