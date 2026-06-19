pipeline {
    agent any

    environment {
        SONAR_TOKEN = credentials('sonar-token')
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/MarriHemalatha/devops-poc-01.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('SonarQube') {
            steps {
                sh '''
                mvn sonar:sonar \
                -Dsonar.projectKey=myapp \
                -Dsonar.host.url=http://<18.225.177.48>:9000 \
                -Dsonar.login=$SONAR_TOKEN
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t myapp .'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy image myapp'
            }
        }

        stage('Deploy') {
            steps {
                sh 'docker run -d -p 8081:80 myapp'
            }
        }
    }
}
