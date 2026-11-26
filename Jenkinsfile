
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
                sh '''
                docker tag sampleapp dineshdockercode/sampleapp:latest
                docker push dineshdockercode/sampleapp:latest
                '''
            }
        }
    }
}
