def credential = 'github-ivankalan12'
def server = 'kel2@103.183.74.32'
def directory ='wayshub-frontend'
def branch = 'main'
def images = 'ivankalan12/wayshub-fe'
def rname = "origin"
def cont = 'wayshub-fe'

pipeline {
    agent any

    stages {
        stage('Repository Pull') {
            steps {
                sshagent([credential]){
                        sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                        echo "Pulling Wayshub Backend Repository"
                        cd ${directory}
                        docker container stop ${cont}
                        git pull ${rname} ${branch}
                        exit
                        EOF"""
                }    
            }    
        }    
        stage('Building Docker images'){
            steps{
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
                    cd ${directory}
                    docker build -t ${images}:${env.BUILD_ID} .
                    exit
                    EOF"""
                }    
            }    
        }    
        stage('Deploying Image'){
            steps{
                sshagent([credential]){
                    sh """ssh -o StrictHostKeyChecking=no ${server} << EOF
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
                        docker image push ${images}:${env.BUILD_ID}-latest
                        exit
                    EOF"""
                }
            }
        }
    }
}
