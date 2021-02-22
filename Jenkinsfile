pipeline {
    agent any

        parameters { 
        string(name: 'DOCKER_IMAGE_NAME', defaultValue: 'bookjs', description: 'Docker Image')
        string(name: 'DOCKER_CONTAINER_NAME', defaultValue: 'bookjs', description: 'Docker Container Name')
        string(name: 'DOCKER_PORT', defaultValue: '3000', description: 'Docker Port')

    }

    // Apaga os dados do Workspace usando o plugin Workspace Cleanup Plugin
    stages {
        stage ('CleanResources') {
            agent any
            steps
            {
                cleanWs()
            }
        }

    // Remove a imagem e container anterior
       stage('Remove Previous Image and Container') {
            steps {
                sh 'docker rm --force "$CONTAINER_NAME"'
                sh 'docker rmi --force "$IMAGE_NAME"'

            }
        }

    //Maven Clean package
        stage('Build Maven') { 
            steps {
                sh 'mvn -version'
                sh 'mvn clean install' 
            }
        }    

    // Criar a imagem docker
        stage ('Build Docker Image') {
                agent any
                steps {
                        sh 'docker build -t "${DOCKER_IMAGE_NAME}" .'
                }
            }
    // Inicia o container
            stage ('Run Docker Container') {
                agent any
                steps {
                    sh 'docker rm -f "${DOCKER_IMAGE_NAME}"'
                    sh 'docker run -d -p ${DOCKER_PORT}:3000 --name "${DOCKER_CONTAINER_NAME}" "${DOCKER_IMAGE_NAME}"'
                }
            }
        }
    }