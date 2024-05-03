pipeline {
    agent any

    tools { nodejs "node" }

    stages {
        stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/sanaabahcine/Deploy-NodeApp']])
                }
            }
        }

        stage('Node JS Build') {
            steps {
                script {
                    
                    sh 'npm install'
                }
            }
        }

        stage('Build Node JS Docker Image') {
            steps {
                script {
                    sh 'docker build -t sanaeabahcine371/node-app-1.0 .'
                }
            }
        }

        stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: 'devopshintdocker', variable: 'devopshintdocker')]) {
                        sh 'docker login -u sanaeabahcine371 -p ${devopshintdocker}'
                    }
                    sh 'docker push sanaeabahcine371/node-app-1.0'
                }
            }
        }

        stage('Deploying Node App to Kubernetes') {
            steps {
                script {
                    sh "minikube start" // Démarre Minikube s'il ne l'est pas déjà
                    sh "kubectl config use-context minikube" // Configurez kubectl pour utiliser le contexte Minikube
                    sh "kubectl apply -f nodejsapp.yaml" // Appliquer la configuration YAML pour le déploiement sur Minikube
                }
            }
        }
    }
}
