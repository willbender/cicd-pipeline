pipeline {
    agent none
    stages {
        //Build stage to build the app
        stage('Build') { 
            agent  {
                docker {
                    image 'node:7.8.0' 
                    args '-p 3000:3000'
                    reuseNode true
                }
            }
            steps {
                sh 'npm install' 
            }
        }
        //Test stage to run tests for the app
        stage('Test') { 
            agent  {
                docker {
                    image 'node:7.8.0' 
                    args '-p 3000:3000'
                    reuseNode true
                }
            }
            steps {
                sh 'npm test' 
            }
        }
        //Build stage to build the docker images
        stage('Docker Build') { 
            agent  any
            steps {
                script{
                    def image = docker.build("registry:5000/node${env.BRANCH_NAME}:1.0")
                    image.push()
                }
            }
        }
        //Deploy stage to run the docker images
        stage('Deploy') { 
            agent  any
            steps {
                script{
                    sh "docker rm -f node${env.BRANCH_NAME}:1.0"
                    sh "docker image pull registry:5000/node${env.BRANCH_NAME}:v1.0"
                    EXPOSE_PORT=3001
                    if(BRANCH_NAME == 'dev'){
                        EXPOSE_PORT=3002
                    }
                    sh "docker run -d --expose ${EXPOSE_PORT} -p ${EXPOSE_PORT}:3000 --name node${env.BRANCH_NAME}:1.0 registry:5000/node${env.BRANCH_NAME}:v1.0"
                }
            }
        }
    }
}
