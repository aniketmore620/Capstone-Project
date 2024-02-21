pipeline {
    agent any
    tools {
        nodejs 'nodejs'
    }
    stages {
        stage('checkout') {
            steps {
                git branch: 'Dev', url: 'https://github.com/aniketmore620/Capstone-Project.git'
            }
        }
        stage('Build') {
            steps {
                sh 'npm install'
                // sh 'npm run build'
            }
        }
        stage('Test') {
            steps {
                // sh 'npm run test'
                echo "Test"
            }
        }
        stage('Build Image') {
            steps {
                sh 'docker build -t reactimage .'
                sh 'docker tag reactimage:latest moreaniket/dev:latest'
            }
        }
        stage('Docker login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-pwd', passwordVariable: 'PASS', usernameVariable: 'USER')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker push moreaniket/dev:latest'
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    def dockerCmd = 'docker run -itd --name My-first-container -p 8081:5000 moreaniket/dev:latest'

                    echo "SSH Key Home: ${HOME}"  // Debugging information

                    sshagent(['sshkeypair']) {
                        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.17.200 'echo \$HOME; ${dockerCmd}'"
                    }
                }
            }
        }
    }
}
