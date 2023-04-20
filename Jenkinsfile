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
        
        stage('Build') {
            steps {
                sh "docker build -t soulaymendocker123/fastapi-crud:latest ."
            }
        }
      /* stage('Unit Tests') {
            steps {
               sh 'docker-compose exec -i web pytest .'
            }
        } */
        
           stage('Unit Tests') {
            steps {
               sh 'docker-compose run web sh -c "pytest test_main.py --cov=app --cov-report=xml"'
            }
            post {
                always {
                    junit 'test_main.py/**/*.xml'
                }
            }
        }
        
        
        
           stage('SonarQube analysis') {
        steps{
        withSonarQubeEnv('sonarqube-8.9.7') { 
        sh "mvn sonar:sonar"
    }
        }
        }
    //    stage('SonarQube Analysis') {
      //      steps {
        //        withSonarQubeEnv('SonarQube') {
          //          sh "mvn sonar:sonar -Dsonar.host.url=http://192.168.19.128:9000 -Dsonar.login=${env.SONAR_LOGIN}"
            //    }
            //}
        //}
        stage('Docker login') {
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
        }
    }
}
