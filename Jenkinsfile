pipeline {
    agent any

    stages {

        stage('STAGE 1 --> Clone Public Repo') {
            steps {
                echo "📥 Cloning public repo..."
                git branch: 'main', url: 'https://github.com/YOUR_USERNAME/public-docker-deploy.git'
                echo " STAGE 1 succesful "
            }
        }

        stage('SATGE 2 --> Build Docker Image') {
            steps {
                sh '''
                    echo "🐳 Building Docker image..."
                    docker build -t public-nginx-app .
                    echo " STAGE 2 succesful "
                '''
            }
        }

        stage('STAGE 3 --> Run Container') {
            steps {
                sh '''
                    echo "🔥 Stopping old container (if exists)..."
                    docker rm -f public-nginx-container || true

                    echo "🚀 Starting new container..."
                    docker run -d -p 8081:80 --name public-nginx-container public-nginx-app
                    echo " STAGE 3 succesful "
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Container deployed successfully! Visit: http://<YOUR_EC2_PUBLIC_IP>:8081"
        }
        failure {
            echo "❌ Deployment failed — check logs"
        }
    }
}
