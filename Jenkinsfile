pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                script {
                echo "Building"
                sh 'mvn -B -DskipTests clean package -l buildlog.txt'
                def logfile = sh 'tail buildlog.txt'
                publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building", text: "${logfile}")
            }
            }
        }
        stage('Test') {
            steps{
                echo "Testing"
                sh 'mvn test'
            }
            post {
                always {
                    sh 'ls -l target/surefire-reports/'
                    junit checksName: 'Unit Tests', testResults: '**target/surefire-reports/TEST-*.xml', keepLongStdio: true, allowEmptyResults: true
                }
            }
        }

    }
}
