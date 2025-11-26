pipeline {
    agent any

    environment {
        SONAR_ORG = "devopslabshandson"
        SONAR_PROJECT = "devopslabshandson"
    }

    stages {

        stage('STAGE 1 --> Clone Public Repo') {
            steps {
                echo "📥 Cloning public repo..."
                git branch: 'main', url: 'https://github.com/iddhawan01/Application-on-Docker-Container.git'
                echo " STAGE 1 successful "
            }
        }

        stage('STAGE 2 --> SonarCloud Code Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeTesting') {
                    sh '''
                        echo "🔍 Running SonarCloud analysis..."

                        sonar-scanner \
                          -Dsonar.projectKey=${SONAR_PROJECT} \
                          -Dsonar.organization=${SONAR_ORG} \
                          -Dsonar.sources=. \
                          -Dsonar.host.url=https://sonarcloud.io \
                          -Dsonar.login=$SONAR_AUTH_TOKEN
                    '''
                }
            }
        }

        stage('STAGE 3 --> Build Docker Image') {
            steps {
                sh '''
                    echo "🐳 Building Docker image..."
                    docker build -t public-nginx-app .
                    echo " STAGE 3 successful "
                '''
            }
        }

        stage('STAGE 4 --> Run Container') {
            steps {
                sh '''
                    echo "🔥 Stopping old container (if exists)..."
                    docker rm -f public-nginx-container || true

                    echo "🚀 Starting new container..."
                    docker run -d -p 8081:80 --name public-nginx-container public-nginx-app
                    echo " STAGE 4 successful "
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Container deployed successfully! Visit: http://13.235.33.103:8081"
        }
        failure {
            echo "❌ Deployment failed — check logs"
        }
    }
}
