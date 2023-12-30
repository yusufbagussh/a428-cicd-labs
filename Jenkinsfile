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
        withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
            // //ssh version
            // sh 'git remote set-url heroku git@heroku.com:a428-cicd-labs.git'   
            // sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
            // sh 'ssh -v git@heroku.com'// Tes koneksi SSH
            // sh 'GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no" git push heroku HEAD:master -v'
                        // Atur ulang remote Heroku ke HTTPS
            sh 'git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git'
            sh 'git remote -v' // Memeriksa remote yang sudah diatur
            
            // Tambahkan HEROKU_API_KEY ke URL git remote
            sh 'git config http.extraheader "Authorization: Bearer ${HEROKU_API_KEY}"'

            // Push ke Heroku menggunakan HTTPS dan otentikasi dengan API key
            sh 'git push heroku HEAD:master -v'
        }
    }
}