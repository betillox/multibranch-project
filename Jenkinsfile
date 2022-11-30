pipeline {
    agent {
        label 'linux-worker'
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
                withCredentials([string(credentialsId: 'docker-pwd-id', variable: 'docker-credentials')]) {
                    sh 'docker login -u betillo -p ${dockerpwd}'
                    sh 'docker image push betillo/spring-webapp:latest'
                }
            }
        }
    }
}
