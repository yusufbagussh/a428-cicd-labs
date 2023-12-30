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
        // stage('Test') {
        //     sh './jenkins/scripts/test.sh'
        // }
        // stage('Manual Approve'){
        //     sh './jenkins/scripts/deliver.sh'
        //     // input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
        // }
    }

    stage('Deploy') {
        def herokuApp = 'a428-cicd-labs'
        withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
            // sh './jenkins/scripts/kill.sh'
            sh 'heroku git:remote -a a428-cicd-labs || echo "Failed to set Heroku remote"'
            sh 'git remote -v' // Untuk memeriksa remote yang sudah diatur
            sh 'git push heroku HEAD:master -v' // Tambahkan '-v' untuk verbose output
        }
    }
}