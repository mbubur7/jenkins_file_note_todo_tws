pipeline {
    agent { label 'dev-agent-server' }
    triggers{ cron('H/02 * * * *') }
    
    stages{
        stage('Code'){
            steps {
                
                git url: 'https://github.com/mbubur7/node-todo-cicd.git', branch: 'master'
            }
        }
        stage('Build and Test'){
            steps {
                sh 'sudo docker build . -t mbubur/node-todo-cicd:latest' 
            }
        }
        stage('Docker hub Login '){
            steps{
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUsername')]) {
                    sh "docker login -u ${env.dockerHubUsername} -p ${env.dockerHubPassword}"
                    sh "docker push mbubur/node-todo-cicd:latest "
                }
            }
        }
        
        stage('Deploy'){
            steps {
                sh 'sudo docker-compose down && docker-compose up -d'
            }
        }
    }
}
