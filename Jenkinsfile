pipeline {
    agent any
    environment {
        NETBOX_TOKEN = '2d13b3e7d3bdff08860575e61619046aab51cd40'
        NETBOX_API   = 'http://192.168.1.7:8000/api/'
    }
    #pawel
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', 
                    url: 'https://github.com/baltah666/pawel.git', 
                    credentialsId: 'github-cred'
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''#!/bin/bash
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
                if [ -f requirements.txt ]; then
                    pip install -r requirements.txt
                fi
                ansible-galaxy collection install netbox.netbox
                '''
            }
        }
        stage('Run Automation Script') {
            steps {
                sh '''#!/bin/bash
                . venv/bin/activate
                ansible-playbook -i netbox_inv_02.yml generate_config.yml
                '''
            }
        }
    }
}
