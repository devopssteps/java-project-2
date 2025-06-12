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
        stage('Login to dockerhub and push the image') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                sh 'docker push devopssteps/myapp:latest'
            }
        }
        stage('deploy to kubernetes') {
            steps {
                script {
                    sh './k8s/deploy.sh'
                    sh 'kubectl rollout restart deployment.apps/myapp-deployment' // this force to deployment with the new docker image
                }
            }
        }
       
    }
}

