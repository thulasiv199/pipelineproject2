pipeline {
    agent any

    environment {
        EC2 = "ubuntu@18.191.187.235"
        APP_DIR = "/home/ubuntu/simple-java-app"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main'
                    url: 'https://github.com/thulasiv199/pipelineproject2.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Unit Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit testResults: '**/target/surefire-reports/*.xml', allowEmptyResults: true
                }
            }
        }

        stage('Deploy to EC2') {
            steps {
                sshagent(['d16647f4-f206-4c43-ac93-10b89626c82c']) {
                    sh """
                        ssh -o StrictHostKeyChecking=no ${EC2} "mkdir -p ${APP_DIR} && pkill -f app.jar || true"
                        scp -o StrictHostKeyChecking=no target/simple-java-app-1.0.0.jar ${EC2}:${APP_DIR}/app.jar
                        ssh -o StrictHostKeyChecking=no ${EC2} "nohup java -jar ${APP_DIR}/app.jar > ${APP_DIR}/app.log 2>&1 &"
                    """
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, upload, and deployment succeeded!'
        }
        failure {
            echo 'Something went wrong. Check the logs.'
        }
    }
}