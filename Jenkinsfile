pipeline {
    agent any
    environment {
        registryURI = "https://index.docker.io/v1/"
        registry = "faizanmomin2508/faizan_cloudethix_nginx"
        registryCredential = '01_docker_Hub_creds'
        tag_commit_id = "${env.registry}:${GIT_COMMIT}"  // Define tag_commit_id at the top level so it is available globally
    }
    stages {
        stage('Building Image from Project Dir') {
            steps {
                script {
                    def app = docker.build(tag_commit_id)
                    docker.withRegistry(registryURI, registryCredential) {
                        app.push()
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    def app = docker.image(tag_commit_id)  // Use docker.image to reference the already built image
                    docker.withRegistry(registryURI, registryCredential) {
                        app.push()
                        app.push('latest')
                    }
                }
            }
        }
    }
    post {
        always {
            echo "Deleting the Workspace"
            deleteDir()
        }
    }
}
