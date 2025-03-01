

pipeline {
    agent any
    
    tools{
        jdk 'jdk17' 
        maven 'maven3'
    }

    stages {
        stage('Compile') {
            steps {
               sh "mvn clean package"
            }
        }
        stage('Docker-Build') {
            steps {
               script{
                withDockerRegistry(credentialsId: 'DockerCred'){
                     sh 'docker build -t spencerxxxwb/boardgame:v1 .'
                    }
                }
            }
        }
        stage('Trivy') {
            steps {
               sh ' trivy image --format table -o report.html spencerxxxwb/boardgame:v1 '
            }
        }
        stage('Docker push') {
            steps {
              script{
                  withDockerRegistry(credentialsId: 'DockerCred') {
                    sh " docker push spencerxxxwb/boardgame:v1"
                }
              }
            }
        }
    }
}
