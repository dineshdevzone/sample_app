
pipeline {
    agent any

    stages {

        stage('Git Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/dineshdevzone/sample_app.git'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t sampleapp .'
            }
        }

        stage('Docker Push') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-credentials',
                    usernameVariable: 'DOCKER_USER',
                    passwordVariable: 'DOCKER_PASS'
                )]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login \
                            -u "$DOCKER_USER" --password-stdin

                        docker tag sampleapp \
                            "$DOCKER_USER/sampleapp:latest"

                        docker push "$DOCKER_USER/sampleapp:latest"

                        docker logout
                    '''
                }
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    docker pull dineshdockercode/sampleapp:latest

                    docker rm -f cont2 || true

                    docker rm -f sampleapp-container || true

                    docker run -d \
                        --restart unless-stopped \
                        -p 5000:5000 \
                        --name sampleapp-container \
                        dineshdockercode/sampleapp:latest
                '''
            }
        }
    }
}
