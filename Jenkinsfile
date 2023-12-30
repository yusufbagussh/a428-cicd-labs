node {
    stage('Print Heroku Path') {
        sh 'echo $PATH'
        sh 'which heroku || echo "Heroku CLI not found"'
        sh 'heroku --version'
    }

    stage('Prepare') {
        def newPath = "/usr/local/bin/heroku"
        env.PATH = "${env.PATH}:${newPath}"
        // Login ke Heroku
        sh 'heroku login -i' // -i flag untuk otentikasi interaktif, atau gunakan environment variable untuk otentikasi
        withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
            sh 'heroku container:login' // Login ke registry Heroku (jika menggunakan container)
            sh 'heroku container:push -a a428-cicd-labs web'
            sh 'heroku container:release -a a428-cicd-labs web' // Deployment ke Heroku dari branch master (sesuaikan dengan branch yang Anda gunakan)
        }
    }

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        stage('Manual Approve'){
            sh './jenkins/scripts/deliver.sh'
            input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
        }
    }
}