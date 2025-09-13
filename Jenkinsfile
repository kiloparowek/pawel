pipeline {
    agent any
    environment {
        NETBOX_TOKEN = 'cf3ce18b328d0f6f8be8a543dfb26f7f873ebfd9'
        NETBOX_API   = 'http://192.168.5.175:8000/api/'
    }
    stages {
        stage('build the application') {
            steps {
                echo "Build"
            }
        }
        stage('test the application') {
            steps {
                echo "Test"
            }
        }

    }
}
