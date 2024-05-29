pipeline {
    agent any
    environment {
        registryURI = "https://registry.hub.docker.com"
        registry = "faizanmomin2508/faizan_cloudethix_nginx"
        registryCredential = "01_docker_Hub_creds"
    }
    stages {
        stage('Building Image from Project Dir') {
            environment {
                registry_endpoint = "${env.registryURI}${env.registry}"
                tag_commit_id = "${env.registry}:${GIT_COMMIT}"
            }
            steps {
                script {
                    def app = docker.build(tag_commit_id)
                    docker.withRegistry(registry_endpoint, registryCredential) {
                        app.push()
                    }
                }
            }
        }
        stage('Deploy Image') {
            environment {
                registry_endpoint = "${env.registryURI}${env.registry}"
            }
            steps {
                script {
                    def app = docker.build(tag_commit_id)
                    docker.withRegistry(registry_endpoint, registryCredential) {
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