pipeline {
        agent any
        stages {

                stage('Checkout') {
                    steps {
                        git url: 'https://github.com/kodekloudhub/jenkins-project.git', branch: 'main'
                        sh "ls -ltr"
                    }
                }
                stage('Setup') {
                     sh '''
                        python3 -m venv myenv
                        source myenv/bin/activate
                          pip install -r requirements.txt
                     '''
                }
                stage('Test') {
                    steps {
                        sh "pytest"
                        sh "whoami"
                    }
                }
                
                }
        }
