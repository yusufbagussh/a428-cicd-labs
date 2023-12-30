node {
    stage('Print Heroku Path') {
        sh 'echo $PATH'
        sh 'which heroku || echo "Heroku CLI not found"'
        sh 'heroku --version'
    }

    stage('Prepare') {
        def newPath = "/usr/local/bin/heroku"
        env.PATH = "${env.PATH}:${newPath}"
    }

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }
    }

    stage('Deploy') {
        def herokuApp = 'a428-cicd-labs'
        withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
            // sh './jenkins/scripts/kill.sh'
            sh 'git remote set-url heroku git@heroku.com:${herokuApp}.git'   
            sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
              // Melakukan push menggunakan SSH dan variabel lingkungan untuk otentikasi
            sh 'GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no" git push heroku HEAD:master -v'
        }
    }
}