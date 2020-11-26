pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building"
                publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building")
            }
        }
        stage('Test') {
            steps{
                echo "Testing"
            }
        }

    }
}
