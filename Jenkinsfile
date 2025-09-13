pipeline {
    agent any
    environment {
        NETBOX_TOKEN = 'cf3ce18b328d0f6f8be8a543dfb26f7f873ebfd9'
        NETBOX_API   = 'http://192.168.5.175:8000/api/'
    }
    stages {
        stage('build the application') {
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
        stage('test the application') {
            steps {
                echo "Test"
            }
        }

    }
}
