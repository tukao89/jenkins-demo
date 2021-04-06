pipeline {
    agent any
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
        stage('Build image') {
            steps {
                sh 'docker build -t nodejs-todolist .'
                sh 'nohup node index.js &'
                sh 'npm test'
            }
        }
        stage('Integration') {
            steps {
                junit 'test.xml'
            }
        }
    }
    post { 
        always { 
            emailext body: '${DEFAULT_CONTENT}', subject: '${DEFAULT_SUBJECT}', to: 'tu.phunganh@gmail.com'
        }
    }
}
