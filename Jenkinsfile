pipeline {
    agent any
    environment {
        image_name = 'kylekingcdn/caddy-dns-route53'
        image_tag = 'latest'
        registry_endpoint = ''
        registry_credentials = 'dockerhub-credentials'
    }
    stages {
        stage('Build Images') {
            steps {
                script {
                    sh label: "Build Image ", script: """
                        docker buildx build -t ${image_name}:${image_tag} --force-rm --no-cache --pull .
                    """
                }
            }
        }
        stage('Push Images') {
            steps {
                script {
                    sh label: "Push Image ", script: """
                        docker push ${image_name}:${image_tag}
                    """
                }
            }
        }
    }
    post {
        cleanup {
            sh label: "Remove image ", script: """
                docker image rm ${image_name}:${image_tag} || true;
            """
            sh label: "Prune images ", script: """
                docker image prune --force;
            """
            cleanWs()
        }
    }
}
