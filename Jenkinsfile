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
        withCredentials([usernamePassword(credentialsId: 'heroku-credential-jenkins', usernameVariable: 'HEROKU_LOGIN', passwordVariable: 'HEROKU_API_KEY')]) {
            sh '''
                echo 'machine git.heroku.com' > $HOME/.netrc
                echo 'login $HEROKU_LOGIN' >> $HOME/.netrc
                echo 'password $HEROKU_API_KEY' >> $HOME/.netrc
                chmod 600 $HOME/.netrc
            '''
            git push heroku HEAD:master
        }
    }
}