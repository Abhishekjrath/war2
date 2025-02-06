pipeline {
    agent any

    tools{
        maven 'maven'
    }

    stages{
        stage('Check & remove container'){
            steps{
                script{
                    def containerExists = sh(script: "docker ps -q -f name=abhi", returnStdout: true).trim()
                    if (containerExists) {
                    sh "docker stop abhi"
                    sh "docker rm abhi"
                    }
                }
            }
        }
        stage('Build package'){
            steps{
                sh 'mvn clean package'
            }
        }
        stage('Create image'){
            steps{
                sh 'sudo docker build -t app /var/lib/jenkins/workspace/webapp/'
            }
        }
        stage('Assign tag'){
            steps{
                sh 'docker tag app abhishek2abc/web'
            }
        }
        stage('Push to dockerhub'){
            steps{
                sh 'echo "Abhi@2001" | docker login -u "abhishek2abc" --password-stdin'
                sh 'docker push abhishek2abc/web'
            }
        }
        stage('Remove images'){
            steps{
                sh 'docker rmi -f $(docker images -q)'
            }
        }
        stage('Pull image from DockerHub'){
            steps{
                sh 'docker pull abhishek2abc/web'
            }
        }
        stage('Run a container'){
            steps{
                sh 'docker run -it -d --name abhi -p 8081:8080 abhishek2abc/web'
            }
        }
    }
    post {
        success {
            echo 'Deployment successful'
        }
        failure {
            sh 'docker rm -f abhi'
        }
        always{
            echo 'Deployed'
        }
    }

}