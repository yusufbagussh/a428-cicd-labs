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

    stage('Heroku Login') {
        withCredentials([usernamePassword(credentialsId: '3085c2ad-00f9-4ca7-a80c-aa15c11a7ddb', passwordVariable: 'HEROKU_API_KEY', usernameVariable: 'HEROKU_EMAIL')]) {
            sh "echo $HEROKU_API_KEY | heroku login --username=$HEROKU_EMAIL --password-stdin"
        }
    }

    stage('Deploy') {
        sh "git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git"   
        sh 'cat $HOME/.netrc'   
        sh 'git push heroku HEAD:master'
    }
}