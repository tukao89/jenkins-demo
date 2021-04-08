pipeline {
    agent {
        label 'jenkin-slave-node'
    }
    environment {
        scannerHome = tool 'sonarQube'
    }
    stages {
        stage('Clone code') {
            steps {
                git branch: 'main', url: 'https://github.com/tukao89/jenkins-demo.git'
            }
        }
        stage('Test app'){
          steps {
            sh 'npm install'
            sh 'nohup node index.js &'
            sh 'npm test'
            junit 'test.xml'
          }
        }
        stage('Sonar Qube'){
            steps{
                withSonarQubeEnv('sonarQubeServer') {
                    sh "${scannerHome}/bin/sonar-scanner"
                }
            }
        }
        stage("Quality Gate") {
            steps {
              timeout(time: 1, unit: 'HOURS') {
                waitForQualityGate abortPipeline: true
              }
            }
        }
    }
    post { 
        success{
            sh 'docker build -t jenkins-demo .'
        }
        always { 
            emailext body: '${DEFAULT_CONTENT}', subject: '${DEFAULT_SUBJECT}', to: 'tu.phunganh@gmail.com'
        }
    }
}
