pipeline {
    agent {
        node {
            label "maven"
        }
    }
    environment {
        PATH = "/opt/maven/bin:$PATH"
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
    }
}

