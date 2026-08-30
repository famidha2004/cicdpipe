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

            docker rm -f mypage 2>/dev/null || true

            docker run -d \
                -p 9080:80 \
                --name mypage \
                famidha/myapp:latest

            docker ps
        '''
    }
}
    }

    post {
    always {
        sh 'docker logout || true'
    }

    success {
        echo 'Pipeline completed successfully!'
    }

    failure {
        echo 'Pipeline failed. Check the failed stage.'
    }
}
}
