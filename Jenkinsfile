pipeline {
    agent any

    stages {
        stage('Clone Git Repo') {
            steps {
               git branch: 'main', credentialsId: 'Git_Credentials', url: 'https://github.com/lokeshdangii/maven-web-app.git'
            }
        }
        
        stage('Maven Build') {
            steps {
               sh 'mvn clean package'
            }
        }
        
        stage('Create Image') {
            steps {
               sh "docker build -t lokeshdangi/maven-web-app ."
            }
        }
        
        stage('Push Image') {
            steps {
               withCredentials([string(credentialsId: 'Docker-PWD', variable: 'dockerpwd')]) {
                sh "docker login -u lokeshdangi -p ${dockerpwd}"
                sh "docker push lokeshdangi/maven-web-app"
            }
            }
        }
        
        stage('Deploy in K8S') {
            steps {
               sh "kubectl delete deployment mavenwebappdeployment"
               sh "kubectl apply -f k8s-deploy.yml"
            }
        }

    }
}