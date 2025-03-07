pipeline {
    agent any

    stages {
        // Line
        /*
        Line 1
        Line 2
        */
        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }

            steps {
                sh '''
                    echo "Building node app..."
                    node --version
                    npm --version
                    ls -la
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
                    echo "Testing node app..."
                    test -f build/index.html && echo "File exists!"
                    echo "Run npm test..."
                    npm test
                '''
            }
        }

        stage('End to End Test') {
            agent {
                docker {
                    image 'mcr.microsoft.com/playwright:v1.39.0-jammy'
                    reuseNode true
                    //args '-u root:root'
                }
            }

            steps {
                sh '''
                    echo "Testing end-to-end node app..."
                    npm install serve
                    echo "Serving from build..."
                    node_modules/.bin/serve -s build &
                    echo "Wait for few seconds..."
                    sleep 10
                    echo "Starting playwright test..."
                    npx playwright test --reporter=html
                '''
            }
        }
    }

    post {
        always {
            junit 'jest-test-results/junit.xml'
            publishHTML([allowMissing: false, alwaysLinkToLastBuild: false, icon: '', keepAll: false, reportDir: 'playwright-report', reportFiles: 'index.html', reportName: 'Playwright HTML Report', reportTitles: '', useWrapperFileDirectly: true])
        }
    }
}
