pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Pull from GitHub using your credentials
                git branch: 'main', 
                    url: 'https://github.com/baltah666/pawel.git', 
                    credentialsId: 'github-cred'
            }
        }
        stage('Setup Environment') {
            steps {
                sh '''
                # Create virtual environment
                python3 -m venv venv
                source venv/bin/activate

                # Install dependencies
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
                source venv/bin/activate
                # Replace with your actual script name
                ansible-playbook generate_config.yml
                '''
            }
        }
    }
}
