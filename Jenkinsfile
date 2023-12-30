pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Print Heroku Path') {
            steps {
                sh 'echo $PATH'
                sh 'which heroku || echo "Heroku CLI not found"'
                sh 'heroku --version'
            }
        }
        stage('Prepare') {
            steps{
                def newPath = "/usr/local/bin/heroku"
                env.PATH = "${env.PATH}:${newPath}"
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}