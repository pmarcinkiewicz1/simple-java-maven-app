pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building"
                sh 'mvn -B -DskipTests clean package'
                publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building")
            }
        }
        stage('Test') {
            steps{
                echo "Testing"
                sh 'mvn test'
            }
            post {
                always {
                    sh 'ls -l'
                    junit checksName: 'Unit Tests', testResults: 'target/surefire-reports/TEST-com.mycompany.app.AppTest.xml'
                }
            }
        }

    }
}
