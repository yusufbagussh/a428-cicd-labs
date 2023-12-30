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
       withCredentials([sshUserPrivateKey(credentialsId: 'heroku-ssh-key', keyFileVariable: 'SSH_KEY')]){
            // //ssh version
            // sh 'git remote set-url heroku git@heroku.com:a428-cicd-labs.git'   
            // sh 'GIT_SSH_COMMAND="ssh -o StrictHostKeyChecking=no" git push heroku HEAD:master -v'

            // sh "git remote set-url heroku git@heroku.com:a428-cicd-labs.git"
            // sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
            // sh 'ssh -v git@heroku.com'// Tes koneksi SSH
            // // Push to Heroku using SSH Key
            // sh "GIT_SSH_COMMAND='ssh -i \$SSH_KEY -o StrictHostKeyChecking=no' git push heroku HEAD:master -v"
            
            // Memulai ssh-agent dan menambahkan kunci privat
            sshagent([SSH_KEY]) {
                // Mengatur remote ke Heroku
                sh 'git remote set-url heroku git@heroku.com:your-app-name.git'
                sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
                // sh 'ssh -v git@heroku.com'// Tes koneksi SSH
                // Melakukan git push menggunakan ssh-agent
                sh 'git push heroku HEAD:master -v'
            }

            // // Atur ulang remote Heroku ke HTTPS
            // sh 'git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git'
            // sh 'git remote -v' // Memeriksa remote yang sudah diatur
            // // Tambahkan HEROKU_API_KEY ke URL git remote
            // sh 'git config http.extraheader "Authorization: Bearer ${HEROKU_API_KEY}"'
            // sh 'git config --global credential.helper "!f() { echo username=; echo password=${HEROKU_API_KEY}; }; f"'
            // // Push ke Heroku menggunakan HTTPS dan otentikasi dengan API key
            // sh 'git push heroku HEAD:master -v'

            // sh 'git push https://:ddbc1eed-6a16-4b67-ae5e-8a880411e809@git.heroku.com/a428-cicd-labs.git HEAD:master -v'
        }
    }
}