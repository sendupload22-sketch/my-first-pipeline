pipeline {
    agent any
    stages {
        stage('Hello') {
            steps {
                echo 'Hello from Jenkins!'
                sh 'date'
            }
        }
        stage('Success') {
            steps {
                echo 'Pipeline completed successfully!'
            }
        }
    }
}
