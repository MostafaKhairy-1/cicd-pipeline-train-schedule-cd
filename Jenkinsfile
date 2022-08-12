pipeline {
    agent any

    stages{
        stage('build'){
            steps{
                echo "build stage"
                sh "./gradlew build"
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'

            }
        }

        stage ('staging'){
            when{
                branch 'master'
            }
            steps{
                withCredentials([usernamePassword(credentialsId: 'webserver_login', usernameVariable: 'USERNAME', passwordVariable: 'USERPASS')])
                sshPublisher(
                    continueOnError: false,
                    failOnError: true,
                    publishers:[
                       configName: "staging",
                       sshCredentials: [
                           username: '$USERNAME',
                           encryptedPassphrase: '$USERPASS'
                       ],

                       transfers:[
                           sourceFiles: 'dist/trainSchedule.zip',
                           removePrefix: 'dist',
                           remoteDirectory: '/tmp',
                           execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'
                       ]
                    ]
                ) 


            
        }
    }

 }

}
