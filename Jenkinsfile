pipeline {
    agent any
    stages {
        stage('build && SonarQube analysis') {
            steps {
                script {
                withSonarQubeEnv('sonarqube') {                   
                        sh 'mvn clean package sonar:sonar -DjarName="$BUILD_NUMBER" |tee buildlog.txt'
                        def url = sh(returnStdout: true, script: 'grep -oE "(http|https)://(.*)" /tmp/log |grep api').trim()
                        sh "wget $url -O jsonlog"
                        def log = sh(returnStdout: true, script: 'tail buildlog.txt').trim()
                        publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building", text: "${log}")
                        
                }
            }
        }
        }
        stage('Test') {
            steps{
                script{
                echo "Testing"
                sh 'mvn test |tee testlog.txt'
                }
            }
            post {
                always {
                    script{
                    def testlog = sh(returnStdout: true, script: 'tail testlog.txt').trim()
                    junit checksName: 'Unit Tests', testResults: '**target/surefire-reports/TEST-*.xml', keepLongStdio: true, allowEmptyResults: true
                    publishChecks(name: "Unit Tests", status: "COMPLETED", summary: "Building", text: "${testlog}")
                    }
                }
            }
        }
        stage('Upload Artifacts'){
            steps{
                script{
                    def server = Artifactory.server 'artifactory'
                    def uploadSpec = """{
                            "files": [{
                            "pattern": "target/*.jar",
                            "target": "java-app"
                            }]
                        }"""
                    server.upload(uploadSpec)
                    publishChecks(name: "Upload Artifacts", status: "COMPLETED", summary: "Aritacts uploaded")
                }
            }
        } 
        

    }
}
