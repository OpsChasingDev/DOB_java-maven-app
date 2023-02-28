pipeline {
    agent any
    stages {
        stage("copy files to ansible server") {
            steps {
                script {
                    echo "copying all necessary files to ansible control server"
                    sshagent(['ansible-server-key']) {
                        sh "scp -o StrictHostKeyChecking=no ansible/* root@206.81.7.158:/root"
                        
                        // this part allows us to reference the contents of the private key as well as the pem file itself
                        withCredentials([sshUserPrivateKey(credentialsId: "ec2-server-key-for-jenkins-ansible", keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                            // this specific syntax prevents groovy interpreter from exposing the $keyfile var to the command line
                            sh 'scp $keyfile root@206.81.7.158:/root/ssh-key.pem'
                        }
                    }
                }
            }
        }
        stage("configure ec2 instances with ansible playbook") {
            steps {
                script {
                    // need to have jenkins invoke command on the ansible controller server
                    echo "calling ansible to configure ec2 instances"
                    def remote = [:]
                    remote.name = "ansible-server"
                    remote.host = "206.81.7.158"
                    remote.allowAnyHosts = true

                    withCredentials([sshUserPrivateKey(credentialsId: "ansible-server-key", keyFileVariable: 'keyfile', usernameVariable: 'user')]) {
                        remote.user = user
                        remote.identityFile = keyfile
                        // sshCommand comes from the jenkins plugin called "ssh pipeline steps"
                        sshCommand remote: remote, command: "ls -l"
                    }
                }
            }
        }
    }   
}