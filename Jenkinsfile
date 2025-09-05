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
        stage('Test Integration') {
            steps {
                echo "Jenkins ↔ GitHub integration is working!"
                sh 'ls -la $WORKSPACE'
            }
        }
    }
}
