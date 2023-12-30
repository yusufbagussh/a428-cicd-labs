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

    stage('Deploy') {
        sh "git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git"   
        sh 'cat $HOME/.netrc'   
        sh 'git push heroku HEAD:master'
    }
}