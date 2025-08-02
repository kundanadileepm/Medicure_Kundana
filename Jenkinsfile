pipeline {
    agent any

    stages {
        stage('Git checkout') {
            steps {
                echo 'Cloning the repo'
                git branch: 'main', url: 'https://github.com/kundanadileepm/Medicure_Kundana.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Packaging the code'
                sh 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                echo 'Building Docker image'
                sh 'docker build -t kundanadileepm/medicure-app:latest .'
            }
        }

        stage('Docker Login & Push') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh 'docker tag kundanadileepm/medicure-app:latest kundanadileepm/medicure-app:v1'
                    sh 'docker push kundanadileepm/medicure-app:v1'
                }
            }
        }

        stage('Deploy using K8s') {
            steps {
                sh 'kubectl apply -f kubernetesfile.yml'
                sh 'kubectl get svc'
            }
        }
    }
}
