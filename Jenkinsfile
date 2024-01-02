node {
    // docker.image('node:16-buster-slim').inside('-p 3000:3000') {
    //     stage('Build') {
    //         sh 'npm install'
    //     }
    //     stage('Test') {
    //         sh './jenkins/scripts/test.sh'
    //     }
    // }
    stage('Deploy to Heroku') {
        // Mengasumsikan HEROKU_API_KEY sudah ditetapkan sebagai credentials di Jenkins
        withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
            // Set Heroku API key sebagai environment variable untuk digunakan oleh Heroku CLI
            sh 'export HEROKU_API_KEY=$HEROKU_API_KEY'
            // Tambahkan remote Heroku jika belum ada
            sh 'git remote | grep heroku || heroku git:remote -a a428-cicd-labs'
            // Push ke Heroku menggunakan Heroku API key
            sh "git push https://apikey:$HEROKU_API_KEY@git.heroku.com/a428-cicd-labs.git HEAD:master"
        }
    }
}