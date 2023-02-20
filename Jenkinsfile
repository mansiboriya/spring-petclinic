pipeline {
    agent any
    tools{
        maven '3.9.0'
    }
    stages{
        stage('Get Code and Build App'){
            steps{
                // Connect to github repo
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mansiboriya/spring-petclinic']])

                // compile and test the app
                sh 'docker build -t springpetclinic:test -f Dockerfile.test .'
                sh 'docker run springpetclinic:test'
            }
        }
        
        stage('Build & Push to JFrog'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'jfrogartifactorycredentials', variable: 'jfrogartifactorypwd')]) {
                        // Build the docker Image
                        sh 'docker build -t springpetclinic:latest .'

                        // Login to JFrog Artifactory
                        sh 'docker login -u mansiboriya@gmail.com -p ${jfrogartifactorypwd} projectjfrog.jfrog.io'

                        // Tag the image
                        sh 'docker tag springpetclinic:latest projectjfrog.jfrog.io/docker/springpetclinic:latest'

                        // Push the Image
                        sh 'docker push projectjfrog.jfrog.io/docker/springpetclinic:latest'
                    }
                }
            }
        }
    }
}