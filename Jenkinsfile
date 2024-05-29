pipeline {
    agent any
    environment {
        registryURI = "https://index.docker.io/v1/"
        registry = "faizanmomin2508/faizan_cloudethix_nginx"
        registryCredential = '01_docker_Hub_creds'
    }
    stages {
        stage('Building Image from Project Dir') {
            environment {
                tag_commit_id = "${env.registry}:${GIT_COMMIT}"
            }
            steps {
                script {
                    def app = docker.build(tag_commit_id)
                    docker.withRegistry(env.registryURI, registryCredential) {
                        app.push()
                    }
                }
            }
        }
        stage('Deploy Image') {
            steps {
                script {
                    def app = docker.build(tag_commit_id)
                    docker.withRegistry(env.registryURI, registryCredential) {
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
