pipeline {
    agent {
        label 'jenkin-slave-node'
    }
    stages {
        stage('Clone code') {
            steps {
                git branch: 'main', url: 'https://github.com/tukao89/jenkins-demo.git'
            }
        }
        stage('Start app'){
          steps {
            sh 'npm install'
          }
        }
        stage('Integration') {
            steps {
                sh 'rm test.xml'
                junit "${WORKSPACE}/test-results/*.xml"
            }
        }
    }
    post { 
        success{
            steps {
                sh 'docker build -t jenkins-demo .'
                sh 'nohup node index.js &'
                sh 'npm test'
            }
        }
        always { 
            emailext body: '${DEFAULT_CONTENT}', subject: '${DEFAULT_SUBJECT}', to: 'tu.phunganh@gmail.com'
        }
    }
}
