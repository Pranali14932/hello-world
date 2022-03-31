pipeline {
    agent any
    tools {
        maven 'Maven' 
    }
    environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
    }
    stages {
        
        
         stage ('checkout') {
            steps {
            checkout([$class: 'GitSCM', branches: [[name: '*/main']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/Pranali14932/hello-world']]])
            }
        }
        
        stage ('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Docker Build') {
            steps {
                script {
                    docker.build("omjai1010/hello-world:${TAG}")
                }
            }
        }
	    stage('Pushing Docker Image to Dockerhub') {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_credential') {
                        docker.image("omjai1010/hello-world:${TAG}").push()
                        docker.image("omjai1010/hello-world:${TAG}").push("latest")
                    }
                }
            }
        }
        stage('Deploy'){
            steps {
                sh "docker stop hello-world | true"
                sh "docker rm hello-world | true"
                sh "docker run --name hello-world -d -p 9004:8080 omjai1010/hello-world:${TAG}"
            }
        }
    }
}
