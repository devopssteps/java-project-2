pipeline {
    agent {
        node {
            label "maven"
        }
    }
    environment {
        PATH = "/opt/maven/bin:$PATH"
        DOCKERHUB_CREDENTIALS = credentials('docker-hub-credential')

    }


    stages {
        stage('build') {
            steps {
                sh 'mvn clean deploy' //-Dmaven.test.skip=true
            }
        }
        stage('Build Docker Image') {
            steps {
                sh 'docker build -t devopssteps/myapp:latest .'
            }
        }
        stage('Login to dockerhub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push to dockerhub') {
            steps {
                sh 'docker push devopssteps/myapp:latest'
            }
        }
    }
}

