pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
    }
    stage("staging") {

            when{
                branch  "master"
            }
            steps {
                withCredentials([usernamePassword(credentialsID: 'webserver_login',usernameVariable: 'userName', passwordVariable: 'userPassword' )])
                sshPublisher (
                     continueOnError : true,
                        failOnError : true,
                    publisher :[
                        configName : "Staging",
                        sshCredentials :
                            username : "$userName",
                            encryptedPassphrase : "$userPassword",

                    ],
                    transfer : [
                        sourceFiles : "dist/trainSchedule.zip",
                        removePrefix : "dist/",
                        remoteDirectory : "/tmp",
                         execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'

                    ]

                )
                
                
            }

        }
    }
    }
}
stage("Production") {

            when{
                branch  "master"
            }
            steps {
                withCredentials([usernamePassword(credentialsID: 'webserver_login',usernameVariable: 'userName', passwordVariable: 'userPassword' )])
                sshPublisher (
                     continueOnError : true,
                        failOnError : true,
                    publisher :[
                        configName : "Production",
                        sshCredentials :
                            username : "$userName",
                            encryptedPassphrase : "$userPassword",

                    ],
                    transfer : [
                        sourceFiles : "dist/trainSchedule.zip",
                        removePrefix : "dist/",
                        remoteDirectory : "/tmp",
                         execCommand: 'sudo /usr/bin/systemctl stop train-schedule && rm -rf /opt/train-schedule/* && unzip /tmp/trainSchedule.zip -d /opt/train-schedule && sudo /usr/bin/systemctl start train-schedule'

                    ]

                )
                
                
            }

        }
    }
    }
}
}
