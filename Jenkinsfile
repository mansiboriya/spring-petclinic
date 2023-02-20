pipeline {
    agent any
    tools{
        maven '3.9.0'
    }
    stages{
        stage('Get Code and Build App'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mansiboriya/spring-petclinic']])
                sh 'docker build -t springpetclinic:test -f Dockerfile.test .'
                sh 'docker run springpetclinic:test'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'jfrogartifactorycredentials', variable: 'jfrogartifactorypwd')]) {
                        sh 'docker build -t springpetclinic:latest .'
                        sh 'docker login -u mansiboriya@gmail.com -p ${jfrogartifactorypwd} projectjfrog.jfrog.io'
                        sh 'docker tag springpetclinic:latest projectjfrog.jfrog.io/docker/springpetclinic:latest'
                        sh 'docker push projectjfrog.jfrog.io/docker/springpetclinic:latest'

                    }
                }
            }
        }
    }
}