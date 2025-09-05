pipeline {
    agent any
    environment {
        NETBOX_TOKEN = '2d13b3e7d3bdff08860575e61619046aab51cd40'
        NETBOX_API   = 'http://192.168.1.7:8000/api/'
        GIT_CRED_ID  = 'github-cred' // Jenkins credential ID
    }
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/baltah666/pawel.git',
                    credentialsId: "${GIT_CRED_ID}"
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
        stage('Commit and Push Changes') {
            steps {
                // Use Jenkins credentials to push
                withCredentials([usernamePassword(credentialsId: "${GIT_CRED_ID}", usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PASS')]) {
                    sh '''#!/bin/bash
                    git config user.name "Jenkins"
                    git config user.email "jenkins@yourdomain.com"

                    # Check for changes
                    if [ -n "$(git status --porcelain)" ]; then
                        git add .
                        git commit -m "Update generated config files"
                        git push https://${GIT_USER}:${GIT_PASS}@github.com/baltah666/pawel.git main
                    else
                        echo "No changes detected"
                    fi
                    '''
                }
            }
        }
    }
}

