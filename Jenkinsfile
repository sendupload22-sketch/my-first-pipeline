//Chiragsingh
pipeline {
    agent any
    
    environment {
        STABLE_VERSION = ''
    }
    
    stages {
        stage('Backup Previous Version') {
            steps {
                script {
                    // Get previous stable commit
                    STABLE_VERSION = sh(script: 'git log --format="%H" -n 2 origin/main | tail -1 || echo "HEAD~1"', returnStdout: true).trim()
                    echo "✅ Backup version: ${STABLE_VERSION}"
                }
            }
        }
        
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh '''
                    echo "Running tests..."
                    sleep 2
                    # Simulate test failure - remove this line to pass
                    # exit 1
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                script {
                    echo '🚀 Deploying NEW version...'
                    sh '''
                        # Simulate deployment
                        echo "NEW VERSION $(date)" > /tmp/deployed-version.txt
                        echo "Deployment to production ✅"
                    '''
                }
            }
        }
        
        stage('Health Check') {
            steps {
                script {
                    // Simulate health check failure
                    def health = sh(script: 'cat /tmp/deployed-version.txt', returnStdout: true).trim()
                    echo "Health check: ${health}"
                    // Uncomment to test rollback:
                    // error("Health check failed!")
                }
            }
        }
    }
    
    post {
        failure {
            script {
                echo '🔄 AUTO ROLLBACK to previous stable version...'
                sh """
                    echo "ROLLING BACK to ${STABLE_VERSION}"
                    git reset --hard ${STABLE_VERSION}
                    echo "STABLE VERSION restored" > /tmp/deployed-version.txt
                    echo "✅ Rollback complete!"
                """
            }
        }
        success {
            echo '🎉 Deployment successful - promoting to stable!'
            sh 'echo "SUCCESS $(date)" > /tmp/deployed-version.txt'
        }
        always {
            sh 'cat /tmp/deployed-version.txt || echo "No deployment file"'
        }
    }
}
