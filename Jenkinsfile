pipeline {
    agent any
    environment {
        image_name = 'kylekingcdn/caddy-dns-route53'
        image_tag = 'latest'
        architectures = 'linux/arm64/v8,linux/amd64'
        registry_endpoint = ''
        registry_credentials = 'dockerhub-credentials'
    }
    stages {
        stage('Build Images') {
            steps {
                script {
                    docker.withRegistry( '', registry_credentials ) {
                        sh label: "Build Image ", script: """
                            docker buildx create --use && \
                            docker buildx build -t ${image_name}:${image_tag} --platform ${architectures} --force-rm --no-cache --pull --push .
                        """
                    }
                }
            }
        }
    }
    post {
        cleanup {
            sh label: "Prune images ", script: """
                docker image prune --force;
            """
            cleanWs()
        }
    }
}
