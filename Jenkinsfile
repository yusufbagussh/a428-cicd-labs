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
            
            // Memulai ssh-agent dan menambahkan kunci privat
            // sshagent(['heroku-ssh-key']) {
            sshagent([SSH_KEY]) {
                // Mengatur remote ke Heroku
                sh 'git remote set-url heroku git@a428-cicd-labs.git'
                sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
                // sh 'ssh -v git@heroku.com'// Tes koneksi SSH
                // Melakukan git push menggunakan ssh-agent
                sh 'git push heroku HEAD:master -v'
            }

        }
    }
}