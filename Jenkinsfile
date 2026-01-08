pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                echo 'Fetching code from GitHub'
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/my-first-pipeline.git'
            }
        }
        stage('Backup Previous Version') {
            steps {
                script {
                    // Tag previous working version as "stable"
                    sh '''
                        git tag -f stable-backup || true
                        git push origin stable-backup --force || true
                    '''
                    echo '✅ Previous version backed up as stable-backup'
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo "All tests PASSED" || exit 1'  // Replace with real tests
            }
        }
        stage('Deploy') {
            steps {
                script {
                    try {
                        echo '🚀 Deploying new version...'
                        sh '''
                            # Simulate deployment (replace with your deploy command)
                            echo "Deploying v$(date +%s)" > /tmp/app-version.txt
                            echo "✅ DEPLOYMENT SUCCESS"
                        '''
                    } catch (Exception e) {
                        echo "❌ DEPLOYMENT FAILED: ${e.getMessage()}"
                        error "Deployment failed - triggering rollback"
                    }
                }
            }
        }
    }
    post {
        failure {
            stage('Rollback') {
                steps {
                    script {
                        echo '🔄 ROLLING BACK to stable-backup...'
                        sh '''
                            # Revert to last working version
                            git reset --hard stable-backup
                            git clean -fd
                            # Re-deploy stable version
                            echo "Reverted to stable-backup" > /tmp/app-version.txt
                            echo "✅ ROLLBACK COMPLETE"
                        '''
                    }
                }
            }
        }
        success {
            stage('Promote Stable') {
                steps {
                    sh 'git tag -f stable && git push origin stable --force'
                    echo '🎉 New version promoted to stable!'
                }
            }
        }
        always {
            sh 'cat /tmp/app-version.txt || echo "No version file"'
        }
    }
}
