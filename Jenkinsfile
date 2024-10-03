pipeline {
    agent any
    environment {
        DOCKER_USERNAME =  'emiliesh'
        GITHUB_REPO_URL =  'https://github.com/EmieHar/cicd-testing-java-cours'
    }

    tools {
        maven 'maven'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: "${env.GITHUB_REPO_URL}"
            }
        }

        stage('Clean') {
            steps {
                sh 'mvn clean'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("${env.DOCKER_USERNAME}/dockercred1:${env.BUILD_NUMBER}", '.')
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'idJenkins') {
                        docker.image("${env.DOCKER_USERNAME}/dockercred1:${env.BUILD_NUMBER}").push()
                    }
                }
            }
        }

        stage('Notify') {
            steps {
                script {
                    if (currentBuild.result == 'SUCCESS') {
                        echo "Build succeeded!"
                    } else {
                        error "Build failed!"
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
