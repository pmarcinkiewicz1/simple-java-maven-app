pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo "Building"
                sh 'mvn -B -DskipTests clean package'
                def textvar = "Build ok from var"
                echo ${textvar}
                publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building", text: ${textvar})
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
