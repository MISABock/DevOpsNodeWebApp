pipeline {
    agent any

    stages {
        stage('Docker Build & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'DockerHubCred', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        export DOCKER_HOST=tcp://host.docker.internal:2375
                        docker build -t michaelmisa/node-web-app:latest .
                        echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                        docker push michaelmisa/node-web-app:latest
                    '''
                }
            }
        }

        stage('Trigger Render Deployment') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'renderToken', variable: 'KEY')]) {
                        sh "curl https://api.render.com/deploy/$KEY"
                    }
                }
            }
        }        
    }
}
