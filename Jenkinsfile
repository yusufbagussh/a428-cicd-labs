node {
    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
    }
    stage('Deploy to Heroku') {
        // Mengasumsikan HEROKU_API_KEY sudah ditetapkan sebagai credentials di Jenkins
        withCredentials([usernamePassword(credentialsId: 'heroku-credential-jenkins', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_PASSWORD')]) {
            sh """
                echo 'machine git.heroku.com' > ~/.netrc
                echo 'login $HEROKU_LOGIN' >> ~/.netrc
                echo 'password $HEROKU_API_KEY' >> ~/.netrc
                chmod 600 ~/.netrc
                git push heroku master
            """
        }
    }
}