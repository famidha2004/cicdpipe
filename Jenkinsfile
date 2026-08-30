pipeline {
    agent any

    environment {
        DOCKER_USERNAME = "famidha"
        DOCKER_IMAGE = "famidha/myapp"
        DOCKER_TAG = "latest"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/famidha2004/cicdpipe.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $DOCKER_IMAGE:$DOCKER_TAG .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([
                    string(
                        credentialsId: 'dockerhub-token',
                        variable: 'DOCKER_TOKEN'
                    )
                ]) {
                    sh '''
                        echo "$DOCKER_TOKEN" | docker login \
                            --username "$DOCKER_USERNAME" \
                            --password-stdin
                    '''
                }
            }
        }

        stage('Push to Docker Hub') {
            steps {
                sh 'docker push $DOCKER_IMAGE:$DOCKER_TAG'
            }
        }
        stage('pull to machine') {
            steps {
                sh '''
                    docker pull famidha/myapp:latest
                    docker run -d -p 8090:80 -name mypage famidha/myapp
                   '''
            }
        }
    }

    post {
        always {
            sh 'docker logout || true'
        }

        success {
            echo 'Docker image pushed successfully!'
        }

        failure {
            echo 'Docker image push failed!'
        }
    }
}
