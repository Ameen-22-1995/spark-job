pipeline {
    agent any

    environment {
        // Define any environment variables here if needed
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    echo '🔧 Building Docker image...'
                    sh 'docker-compose build'
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                script {
                    echo '🚀 Running Docker container...'
                    sh 'docker-compose up -d'
                }
            }
        }

        stage('Run Spark Job') {
            steps {
                script {
                    echo '📊 Running Spark job...'
                    sh 'docker-compose logs -f spark-job' // Optional: specify service name instead of logging all
                }
            }
        }
    }

    post {
        success {
            script {
                slackSend(
                    channel: '#spark-alerts',
                    color: 'good',
                    message: "✅ Build SUCCESSFUL: ${env.JOB_NAME} [#${env.BUILD_NUMBER}] → ${env.BUILD_URL}"
                )
            }
        }

        failure {
            script {
                slackSend(
                    channel: '#spark-alerts',
                    color: 'danger',
                    message: "❌ Build FAILED: ${env.JOB_NAME} [#${env.BUILD_NUMBER}] → ${env.BUILD_URL}"
                )
            }
        }

        always {
            script {
                echo '🧹 Cleaning up containers...'
                sh 'docker-compose down'
            }
        }
    }
}




   
