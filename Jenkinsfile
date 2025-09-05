pipeline {
    agent any
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
                sh '''
                python3 -m venv venv
                . venv/bin/activate

                pip install --upgrade pip
                if [ -f requirements.txt ]; then
                    pip install -r requirements.txt
                fi
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
    }
}
