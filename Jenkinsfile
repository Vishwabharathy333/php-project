pipeline {
    agent any
    stages {
        stage('Git Clone') {
            steps {
                git url: 'https://github.com/Vishwabharathy333/php-project/', branch: "master"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t $USER/vishwabharathy/php-project:latest ."
                    sh "docker images"
                }
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push $USER/vishwabharathy:latest"
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def dockerrm = "sudo docker rm -f My-first-containe2211 || true"
                    def dockerCmd = "sudo docker run -itd --name My-first-containe2211 -p 8083:80 $USER/vishwabharathy:latest"
                    
                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.2.175 ${dockerrm}"
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.2.175 ${dockerCmd}"
                    }
                }
            }
        }
    }
}
