pipeline {
    agent any

    environment {
        SERVER_IP = credentials('prod-server-ip')
    }
    stages {
        stage('Check User') {
             steps {
                 sh 'whoami'
             }
        }
        stage('Setup') {
            steps {
               sh """
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
              """
            }
        }
        stage('Test') {
            steps {
                sh """
                    . venv/bin/activate
                    pytest
                 """
            }
        }
       stage('Package Code'){
            steps {
                sh 'zip -r myapp.zip ./* -x "*.git*"'
                sh 'ls -lart'
            }
        }
        stage('Deploy to Prod') {
            steps {
                withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'MY_SSH_KEY', usernameVariable: 'username')]) {
                sh """
                    scp -i ${MY_SSH_KEY} -o StrictHostKeyChecking=no myapp.zip ${username}@${SERVER_IP}:/home/ubuntu/
                    ssh -i ${MY_SSH_KEY} -o StrictHostKeyChecking=no ${username}@${SERVER_IP} "
                    unzip -o /home/ubuntu/myapp.zip -d /home/ubuntu/app/;
                    source /home/ubuntu/app/venv/bin/activate;
                    cd /home/ubuntu/app/;
                    pip install -r requirements.txt;
                    sudo systemctl restart flaskapp.service
                "
            """
        }
    }
} 
    }
}