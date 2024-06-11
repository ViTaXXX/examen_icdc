pipeline {
    environment {
        LOGIN = 'USER_DOCKERHUB'
        SSH_CREDENTIALS_ID = 'SSH_KEY3'
    }
    agent any
    stages {
        stage("Imagen") {
            agent {
                docker {
                    image "python:3"
                    args '-u root:root'
                }
            }
            stages {
                stage('repositorio') {
                    steps {
                        git branch: 'main', url: 'https://github.com/ViTaXXX/examen_icdc.git'
                    }
                }
            }
        }
        stage("Crear_la_imagen") {
            stages {
                stage('Construir_imagen') {
                    steps {
                        script {
                            newApp = docker.build("andresdocker77/examen_icdc2:latest")
                        }
                    }
                }
                stage('Subirla') {
                    steps {
                        script {
                            docker.withRegistry('', LOGIN) {
                                newApp.push()
                            }
                        }
                    }
                }
                stage('Borrarla') {
                    steps {
                        sh "docker rmi andresdocker77/examen_icdc2:latest"
                    }
                }
            }
        }
        stage("Desplegar") {
            agent any
            steps {
                script {
                    sshagent(credentials: ['SSH_KEY3']) {
                        sh 'ssh -o StrictHostKeyChecking=no andres@rinnegan.fernandezds.es wget https://raw.githubusercontent.com/ViTaXXX/examen_icdc/main/docker-compose.yaml -O docker-compose.yaml'
                        sh 'ssh -o StrictHostKeyChecking=no andres@rinnegan.fernandezds.es docker compose up -d --force-recreate'
                    }
                }
            }
        }
    }

}
