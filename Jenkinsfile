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
            // Login ke Heroku dengan Heroku CLI
            sh 'heroku login -i'

            sh 'heroku whoami'
            // Tambahkan remote Heroku jika belum ada
            sh 'git remote | grep heroku || heroku git:remote -a a428-cicd-labs'
            // Push ke Heroku
            sh 'git push heroku HEAD:master'
        }
    }
}

// stage('Deploy to Heroku') {
//         withCredentials([usernamePassword(credentialsId: 'heroku-credential-jenkins', usernameVariable: 'HEROKU_LOGIN', passwordVariable: 'HEROKU_API_KEY')]) {
//             sh '''
//                 # Setup .netrc file with Heroku credentials
//                 echo -e "machine git.heroku.com\nlogin $HEROKU_LOGIN\npassword $HEROKU_API_KEY" > $HOME/.netrc
//                 chmod 600 $HOME/.netrc
                
//                 # Debug: Ensure .netrc exists and has correct permissions
//                 ls -la $HOME/.netrc

//                 # Attempt to push to Heroku
//                 git push heroku HEAD:master
//             '''
//         }
//     }