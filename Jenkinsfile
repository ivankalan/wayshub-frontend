def credential = ''
def server = 'kel2@103.183.74.32'
def directory ='wayshub-frontend'
def branch = 'main'
def images = 'ivankaalan12/wayshub-fe:latest'

pipeline{
    agent any
    stages{
        stage('Docker compose down and pulling new images'){
            steps{
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no $ {server} << EOF
                    cd ${directory}
                    docker-compose down
                    docker system prune -f
                    git pull origin ${branch}
                    exit
                    EOF"""
                }    
            }    
        }    
        stage('Building Docker images'){
            steps{
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no $ {server} << EOF
                    cd ${directory}
                    docker build -t ${images} .
                    exit
                    EOF"""
                }    
            }    
        }    
        stage('docker up'){
            steps{
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no $ {server} << EOF
                    cd ${directory}
                    docker-compose up -d
                    exit
                    EOF"""
                }
            }
        }
        stage('Pushing to Docker Hub (ivankalan12)') {
            steps {
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        cd ${directory}
                        docker image push ${images}:${ivankaalan12/wayshub-fe}-latest
                    exit    
                    EOF"""
                }
            }
        }
    }
}
