pipeline {
    agent any

    environment {
        CATALINA_HOME = "/usr/local/tomcat"   // change path if Tomcat is installed elsewhere
    }
     tools {
        maven 'maven-3.9.11'   // the name you configured in Jenkins tools
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
        echo "Starting Spring Boot app..."
        nohup java -jar target/*.jar > app.log 2>&1 &
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
