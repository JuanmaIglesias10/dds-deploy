pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', credentialsId: '591ba79b-2425-4255-87fa-eb74b400c5f2', url: 'https://github.com/JuanmaIglesias10/dds-deploy.git'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def mvn = tool 'mavenTp'
                    withSonarQubeEnv() {
                        sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=sonarQube -Dsonar.projectName='sonarQube'"
                    }
                }
            }
        }

        stage('Deploy Image To Kubernetes') {
            steps {
                script {
                    sshagent(['vm1Credentials']) {
                        sh "ssh jiglesias@192.168.182.128 'echo Conexión exitosa'"
                        sh "ssh jiglesias@192.168.182.128 'docker pull jiglesiass/ddsdeploy'"
                        sh "ssh jiglesias@192.168.182.128 'cd /home/jiglesias/dds-deploy && docker build -t jiglesiass/ddsdeploy .'"
                        sh "ssh jiglesias@192.168.182.128 'docker push jiglesiass/ddsdeploy'"
                        sh "ssh jiglesias@192.168.182.128 'minikube start'"
                        sh "ssh jiglesias@192.168.182.128 'kubectl create deployment deploytp --image=jiglesiass/ddsdeploy'"
                        sh "ssh jiglesias@192.168.182.128 'kubectl expose deployment deploytp --port=80 --type=LoadBalancer'"
                        sh "sleep 20"
                        sh "ssh jiglesias@192.168.182.128 'minikube service deploytp --url'"
                    }
                }
            }
        }
    }
}

