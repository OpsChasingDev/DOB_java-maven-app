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
    }   
}