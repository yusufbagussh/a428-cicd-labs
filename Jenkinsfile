node {
    stage('Print Heroku Path') {
        sh 'echo $PATH'
        sh 'which heroku || echo "Heroku CLI not found"'
        sh 'heroku --version'
    }

    stage('Prepare') {
        def newPath = "/usr/local/bin/heroku"
        env.PATH = "${env.PATH}:${newPath}"
        withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'heroku-credentials', usernameVariable: 'HEROKU_EMAIL', passwordVariable: 'HEROKU_API_KEY']]) {
            // Login ke Heroku (pastikan sudah ada kredensial di Jenkins)
            sh 'heroku container:login' // Login ke Heroku Container Registry
            
            // Push image ke Heroku Container Registry
            sh 'heroku container:push web -a a428-cicd-labs' // Ganti 'a428-cicd-labs' dengan nama aplikasi Anda di Heroku
            
            // Release ke Heroku setelah push
            sh 'heroku container:release web -a a428-cicd-labs' // Ganti 'a428-cicd-labs' dengan nama aplikasi Anda di Heroku
        }
        // withCredentials([string(credentialsId: 'heroku-credentials', variable: 'HEROKU_API_KEY')]) {
        //     sh 'git remote set-url heroku https://$HEROKU_API_KEY@git.heroku.com/a428-cicd-labs.git'
        //     sh 'git push heroku HEAD:master'
        // 
        // }
        
        // sh 'heroku login -i' // -i flag untuk otentikasi interaktif, atau gunakan environment variable untuk otentikasi
        // withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
        //     sh 'heroku container:login' // Login ke registry Heroku (jika menggunakan container)
        //     sh 'heroku git:remote -a a428-cicd-labs' // Set remote Git untuk aplikasi Heroku
        //     sh 'git remote -v' // Set remote Git untuk aplikasi Heroku
        //     sh 'git push heroku HEAD:master'// Deployment ke Heroku dari branch master (sesuaikan dengan branch yang Anda gunakan)
        // }
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