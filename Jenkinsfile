def credential = 'github-ivankalan12'
def server = 'kel2@103.183.74.32'
def directory ='wayshub-frontend'
def branch = 'main'
def images = 'ivankalan12/wayshub-fe:latest'
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
                        cd ${dir}
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
                    docker build -t ${images} .
                    exit
                    EOF"""
                }    
            }    
        }    
        stage('docker up'){
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
    }
}
