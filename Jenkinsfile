pipeline {
    agent any
    environment {
        TEST = 'yes'
        DEPLOY = 'yes'
    }
    stages {
        stage('Test') {
            when { environment name: 'TEST', value: 'yes' }
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']],
     userRemoteConfigs: [[url: 'git@github.com:sdarwin/django-website.git']]])
     sshagent (credentials: ['b8c5efcc-d20a-4b4b-a6c4-090d619dee0e']) {
              sh '''
set -e
. ~/env/bin/activate
.  ~/load_env.sh
pip3 install -r requirements.txt
python manage.py test polls
'''
            }
            }
        }
        stage('Deploy') {
            when { environment name: 'DEPLOY', value: 'yes' }
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/master']],
     userRemoteConfigs: [[url: 'git@github.com:sdarwin/django-website.git']]])
     sshagent (credentials: ['b8c5efcc-d20a-4b4b-a6c4-090d619dee0e']) {
       build 'terraform-production'
            }
        }
    }
}
}

