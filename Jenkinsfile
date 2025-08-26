pipeline {
    agent any

    environment {
        CATALINA_HOME = "/usr/local/tomcat"   // change path if Tomcat is installed elsewhere
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/thulasiv199/pipelineproject2.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                echo "Stopping Tomcat..."
                $CATALINA_HOME/bin/catalina.sh stop || true

                echo "Deploying WAR..."
                cp target/*.war $CATALINA_HOME/webapps/

                echo "Starting Tomcat..."
                $CATALINA_HOME/bin/catalina.sh start
                '''
            }
        }
    }

    post {
        success {
            echo '✅ Build & Deployment successful!'
        }
        failure {
            echo '❌ Build failed. Please check logs.'
        }
    }
}
