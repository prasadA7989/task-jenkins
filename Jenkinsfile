pipeline {
    agent any
        stages {
            stage('Cloning Git Repository') {
                steps {
                    git branch: 'main', changelog: false, poll: false, url: 'https://github.com/prasadA7989/jenkins-tutorial.git'
                    sh 'pwd'
                }
            }
            stage('Build') {
                steps {
                    dir('4-python-jenkins-docker-app'){
                    sh 'python3 -m py_compile app.py'
                    }
                }
            }
            stage('Test') {
                steps {
                    dir('4-python-jenkins-docker-app'){
                    sh 'python3 -m unittest discover tests'
                }
            }
            }
            stage('Docker Build') {
                steps {
                   dir('4-python-jenkins-docker-app'){
                   sh 'docker build -t python-jenkins-app:latest .'
                    sh 'echo "Docker Image Build successfully"'
                   }
                }
            }
            stage('Docker Run') {
                steps {
                   dir('4-python-jenkins-docker-app'){
                   sh 'docker run -d -p 5000:5000 python-jenkins-app:latest'
                   }
                }
            }
            stage('Docker Push') {
                steps {
                   dir('4-python-jenkins-docker-app'){
                   withCredentials([
                       usernamePassword(
                           credentialsId: 'prasad90597-DockerHub', 
                           passwordVariable: 'DOCKER_HUB_PASSWORD', 
                           usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh '''          
                       echo "$DOCKER_HUB_PASSWORD" | docker login -u "$DOCKER_HUB_USERNAME" --password-stdin
                       docker tag python-jenkins-app:latest prasad90597/python-jenkins-app:latest
                        docker push prasad90597/python-jenkins-app:latest'''
                   }
                }
                }
            }
        }
    
}
