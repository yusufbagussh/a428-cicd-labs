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

    // docker.image('node:16-buster-slim').inside('-p 3000:3000') {
    //     stage('Build') {
    //         sh 'npm install'
    //     }
    // }

    stage('Deploy') {
        // sh '''
        // echo "machine api.heroku.com" >> $HOME/.netrc
        // echo "login bagus.herlambang@student.uns.ac.id" >> $HOME/.netrc
        // echo "password 3085c2ad-00f9-4ca7-a80c-aa15c11a7ddb" >> $HOME/.netrc
        // '''
        sh "git remote set-url heroku https://git.heroku.com/a428-cicd-labs.git"      
        sh 'git push heroku HEAD:master'
    }
}