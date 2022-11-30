pipeline {
    agent {
        label 'jenkins-worker'
    }

    stages {

        stage('Compilacion') {
            steps {
               sh 'mvn -D skipTests clean install package'
            }
        }

        stage('Unit test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                   junit 'target/surefire-reports/*.xml'
                }
            }
        }

        stage('Build docker image') {
            steps {
                sh 'docker image build -t spring-webapp .'
            }
        }

        stage('Tag docker image') {
            steps {
                sh 'docker image tag spring-webapp betillo/spring-webapp:latest'
            }
        }

        stage('Upload docker image') {
            steps {
          	sh 'docker login -u betllo -p Guitarra123.'
      	  	sh 'docker image push betillo/spring-webapp:latest'
            }
        }
    }
} 


 
