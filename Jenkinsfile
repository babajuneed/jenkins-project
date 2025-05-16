pipeline {
        agent any
        stages {

                /*stage('Checkout') {
                    steps {
                        git url: 'https://github.com/kodekloudhub/jenkins-project.git', branch: 'main'
                        sh "ls -ltr"
                    }
                }*/
                stage('Setup') {
                    steps {
                     sh '''
                        #!/bin/bash
                        python3 -m venv myenv
                        . myenv/bin/activate
                          pip install -r requirements.txt
                     '''
                    } 
                }
                stage('Test') {
                    steps {
                         sh '''
                         #!/bin/bash
                         . myenv/bin/activate
                         python -m pytest
                         echo "pytest complete"
                        '''
                        }
                }
                
                }
        }
