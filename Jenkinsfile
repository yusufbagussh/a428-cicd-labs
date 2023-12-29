node {
    // stage('Print Heroku Path') {
    //     sh 'echo $PATH'
    //     sh 'which heroku || echo "Heroku CLI not found"'
    //     sh 'heroku --version'
    // }

    // // Asumsi bahwa Heroku CLI sudah terinstall di host Jenkins atau Docker image.
    // stage('Prepare') {
    //     // Menetapkan PATH di awal bisa membantu menghindari masalah saat menjalankan perintah.
    //     def newPath = "/usr/local/bin/heroku"
    //     env.PATH = "${env.PATH}:${newPath}"
    // }

    docker.image('node:16-buster-slim').inside('-p 3000:3000') {
        stage('Build') {
            sh 'npm install'
        }
        stage('Test') {
            sh './jenkins/scripts/test.sh'
        }
        stage('Manual Approve'){
            sh './jenkins/scripts/deliver.sh'
            // input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)'
        }
    }

    stage('Deploy') {
        sh './jenkins/scripts/kill.sh'
        // sh 'heroku git:remote -a a428-cicd-labs'
        // sh 'git push heroku HEAD:master'
    }
}

    //     // Heroku credentials should be set up beforehand.
        // def herokuApp = 'a428-cicd-labs'
        // withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_KEY')]) {
        //     // Menggunakan full path untuk Heroku CLI jika diperlukan.
        //     // sh "heroku git:remote -a ${herokuApp}"

        //     // sh 'echo url=https://:$HEROKU_API_KEY@git.heroku.com/ > ~/.netrc'
        //     // sh 'git remote set-url heroku https://$HEROKU_API_KEY@git.heroku.com/${herokuApp}.git'
        //     // sh 'git push heroku HEAD:master'
            
            
        //     // Mengatur Git untuk menggunakan variabel HEROKU_API_TOKEN sebagai password
        //     sh 'git config --global credential.helper "!f() { echo username=; echo password=$HEROKU_API_KEY; }; f"'
            
        //     // Mengatur remote heroku dengan menggunakan format yang benar
        //     sh 'git remote set-url heroku https://heroku:$HEROKU_API_KEY@git.heroku.com/a428-cicd-labs.git'
            
        //     // Melakukan git push ke Heroku
        //     sh 'git push heroku HEAD:master'
        // }

        //     sh './jenkins/scripts/kill.sh'

        // def herokuApp = 'a428-cicd-labs'
        // withCredentials([usernamePassword(credentialsId: '0cc80853-55f3-401a-a5aa-ae5d05aa8f31', usernameVariable: 'HEROKU_USERNAME', passwordVariable: 'HEROKU_PASSWORD')]) {
        //     // Mengatur remote Heroku dengan nama aplikasi yang benar
        //     sh "git remote set-url heroku https://git.heroku.com/${herokuApp}.git"

        //     // Konfigurasi global credential.helper untuk store
        //     sh 'git config --global credential.helper store'

        //     // Menambahkan kredensial ke store credential Git
        //     sh "echo https://${HEROKU_USERNAME}:${HEROKU_PASSWORD}@git.heroku.com/ > ~/.git-credentials"

        //     // Fetching all branches
        //     sh 'git fetch --all'

        //     // Push ke Heroku
        //     sh 'git push heroku HEAD:master'
        // }

// node {
//     def newPath = "/usr/local/bin/heroku"

//     def updatedPath = "${env.PATH}:${newPath}"

//     // Menetapkan PATH yang diperbarui ke dalam environment
//     env.PATH = updatedPath
//     docker.image('node:16-buster-slim').inside('-p 3000:3000') {
//         stage('Build') {
//             sh 'npm install'
//             sh 'curl https://cli-assets.heroku.com/install.sh | sh'
//         }
//         stage('Test') {
//             sh './jenkins/scripts/test.sh'
//         }
//         stage('Manual Approve'){
//             sh './jenkins/scripts/deliver.sh' 
//             input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 

//         }
//         stage('Deploy') {
//             def herokuApp = 'a428-cicd-labs'
//             withCredentials([string(credentialsId: 'heroku-api-token', variable: 'HEROKU_API_TOKEN')]) {
//                 sh "heroku git:remote -a $herokuApp"
//                 sh 'git push heroku HEAD:main'
//             }
//             sh './jenkins/scripts/kill.sh'
//         }
//     }
// }