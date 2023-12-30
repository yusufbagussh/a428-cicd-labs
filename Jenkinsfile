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
            sh 'heroku login'
            // sh 'heroku container:login'
            sh 'heroku container:push -a a428-cicd-labs web'
            sh 'heroku container:release -a a428-cicd-labs web'
        // withCredentials([usernamePassword(credentialsId: 'heroku-credential', passwordVariable: 'HEROKU_API_KEY', usernameVariable: 'HEROKU_EMAIL')]) {
        //     sh 'git remote set-url heroku https://heroku:$HEROKU_API_KEY@git.heroku.com/a428-cicd-labs.git'
        //     sh 'git push heroku HEAD:master'
        // }
    }

//     stage('Deploy') {
//         sh "git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git"
//         sh 'git push heroku HEAD:master'
//     }
}