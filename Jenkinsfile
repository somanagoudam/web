pipeline{
    agent any
    
    tools{
        jdk 'java-11'
        maven 'maven'
    }
    
    stages{
        stage('Git-checkout'){
            steps{
                git branch: 'dev' , url: 'https://github.com/somanagoudam/web.git'
            }
        }
        stage('Code Compile'){
            steps{
                sh 'mvn compile'
            }
        }
        stage('Code Package'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Build and tag'){
            steps{
                sh 'docker build -t somanagoudam/project .'
            }
        }
        stage('Containerisation'){
            steps{
                sh '''
                docker run -it -d --name c37 -p 9037:8080 somanagoudam/project
                '''
            }
        }
       
        stage('Login to Docker Hub') {
    steps {
        script {
            withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                sh '''
                   echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin
                '''
            }
        }
    }
}

         stage('Pushing image to repository'){
            steps{
                sh 'docker push somanagoudam/project'
            }
        }
        
    }
}
