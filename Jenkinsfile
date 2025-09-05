pipeline {
    agent any

    environment {
        NETBOX_TOKEN = credentials('netbox-generate-configFile')
        NETBOX_API   = 'http://192.168.1.7:8000/api/'
    }

    stages {
        stage('Checkout SCM') {
            steps {
                checkout scm
            }
        }

        stage('Setup Environment') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
                '''
            }
        }

        stage('Run Automation Script') {
            steps {
                sh '''
                    . venv/bin/activate
                    ansible-playbook generate_config.yml
                '''
            }
        }

        stage('Archive Config Files') {
            steps {
                // Adjust the path to where your configs are saved
                archiveArtifacts artifacts: 'configs/**/*', fingerprint: true
            }
        }
    }
}
