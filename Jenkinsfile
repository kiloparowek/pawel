pipeline {
    agent any
    environment {
        NETBOX_TOKEN = '2d13b3e7d3bdff08860575e61619046aab51cd40'
        NETBOX_API   = 'http://192.168.1.7:8000/api/'
    }
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
        stage('Push Generated Configs to GitHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'github-cred', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_TOKEN')]) {
                    sh '''#!/bin/bash
                    # Ensure .git folder is writable
                    chown -R jenkins:jenkins $WORKSPACE
                    chmod -R u+rwX $WORKSPACE/.git

                    # Configure git to use credentials
                    git config user.name "Abdulilah Baltah"
                    git config user.email "baltah666@gmail.com"
                    git remote set-url origin https://$GIT_USER:$GIT_TOKEN@github.com/baltah666/pawel.git

                    # Add and commit changes if there are any
                    git add .
                    if ! git diff --cached --quiet; then
                        git commit -m "Automated config update by Jenkins"
                        git push origin main
                    else
                        echo "No changes to commit."
                    fi
                    '''
                }
            }
        }
    }
}
