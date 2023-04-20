pipeline {
    agent any
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        //SONAR_LOGIN = credentials('sonar-jenkins')
    }
    stages {
        
        stage('docker-compose Build') {
            steps {
                sh "docker-compose up -d --build "
            }
        }
        
        /*stage('Build') {
            steps {
                sh "docker build -t soulaymendocker123/fastapi-crud:latest ."
            }
        }*/
      stage('Unit Tests') {
            steps {
              
              sh 'docker-compose exec -T web pytest .'
            }
        } 

           stage('SonarQube analysis') {
        steps{
        withSonarQubeEnv('sonarqube-8.9.7') { 
            sh 'export PATH="$PATH:/home/soulaymen/SonarQube/sonar-scanner-4.6.0.2311-linux/bin"; sonar-scanner -Dsonar.projectKey=d42eea36dc03ec0f2d8f64f5517e980100d97878'
    }
        }
           }
        /*stage('Docker login') {
            agent any
            steps {
                sh 'echo "login Docker ...."'
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }
        stage('Push Docker Image') {
            steps {
                sh "docker push soulaymendocker123/fastapi-crud:latest"
            }
        }*/
    }
}
