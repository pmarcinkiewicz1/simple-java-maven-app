pipeline {
    agent any
    stages {
        stage('build && SonarQube analysis') {
            steps {
                script {
                withSonarQubeEnv('sonarqube') {                   
                        sh 'mvn clean package sonar:sonar -l sonarlog.txt'
                        def url = sh(returnStdout: true, script: 'grep -oE "(http|https)://(.*)" /tmp/log |grep api').trim()
                        sh "wget $url -O jsonlog"

                
                }
            }
        }
        }
        stage('Build') {
            steps {

                script {
                echo "Building"
                sh 'mvn -B -DskipTests clean package -DjarName="$BUILD_NUMBER" -l buildlog.txt'
                def log = sh(returnStdout: true, script: 'tail buildlog.txt').trim()
                def server = Artifactory.server 'artifactory'
                def uploadSpec = """{
                    "files": [{
                       "pattern": "target/*.jar",
                       "target": "java-app"
                    }]
                 }"""
                server.upload(uploadSpec)
                //publishChecks(name: "Stage Build", status: "COMPLETED", summary: "Building", text: "${log}")
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
                    //junit checksName: 'Unit Tests', testResults: '**target/surefire-reports/TEST-*.xml', keepLongStdio: true, allowEmptyResults: true
                }
            }
        }
        

    }
}
