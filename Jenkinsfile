pipeline {
    agent any
    stages {
        stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    // Optionally use a Maven environment you've configured already
                    
                       sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    
                }
            }
        }
        stage('Build') {
            steps {
                script {
                echo "Building"
                sh 'mvn -B -DskipTests clean package -l buildlog.txt'
                def log = sh(returnStdout: true, script: 'tail buildlog.txt').trim()
                
                echo "${log}"
                publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building", text: "${log}")
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
