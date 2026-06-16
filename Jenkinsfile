pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git branch: 'master',
                    url: 'https://github.com/sairaj-gif/two-tier-flask-app.git'
            }
        }

        stage('Build') {
            steps {
                sh 'docker build -t two-tier-flask-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'mvn test'
            }
        }
        
        stage('Push to Dockerhub') {
    steps {
        withCredentials([usernamePassword(
            credentialsId: 'dockerHubCreds',
            usernameVariable: 'dockerHubUser',
            passwordVariable: 'dockerHubPass'
        )]) {

            sh """
                echo "$dockerHubPass" | docker login -u "$dockerHubUser" --password-stdin

                docker tag two-tier-flask-app:latest $dockerHubUser/two-tier-flask-app:latest

                docker push $dockerHubUser/two-tier-flask-app:latest
            """
        }
    }
}
        stage('Deploy') {
            steps {
               sh 'docker compose up -d --build flask-app '
            }
        }
    }

    post {
        success {
            echo 'Pipeline executed successfully!'
        }
        failure {
            echo 'Pipeline failed!'
        }
    }
}
