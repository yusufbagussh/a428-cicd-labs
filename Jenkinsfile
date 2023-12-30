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
        withCredentials([usernamePassword(credentialsId: 'heroku-credential', passwordVariable: 'HEROKU_API_KEY', usernameVariable: 'HEROKU_EMAIL')]) {
            sh "echo $HEROKU_API_KEY | heroku login --username=$HEROKU_EMAIL --password-stdin"
        }
    }

    stage('Deploy') {
        sh "git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git"
        sh 'git push heroku HEAD:master'
    }
}