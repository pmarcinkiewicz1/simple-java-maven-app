pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building"
                publishChecks(name: 'Stage Build', status: 'in_progress', summary: 'Building')
            }
        }
        stage('Test') {
            steps{
                echo "Testing"
            }
        }

    }
}
