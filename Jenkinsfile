pipeline {
    agent any

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
    }

    post {
            always {
                junit 'test-results/junit.xml'
            }
        }
}
