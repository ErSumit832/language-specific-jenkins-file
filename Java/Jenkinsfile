pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "your-dockerhub-username/java-app"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
        EMAIL_RECIPIENTS = "team@example.com"
    }

    stages {
        stage('Clone') {
            steps {
                git 'https://your-repo-url/java-app.git'
            }
        }

        stage('Trivy Scan') {
            steps {
                sh 'trivy fs . || true'
            }
        }

        stage('OWASP Dependency Check') {
            steps {
                sh './dependency-check/bin/dependency-check.sh --project JavaApp -s . -f HTML -o dependency-check-report'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}").push("latest")
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying Java application...'
                // Your deployment logic
            }
        }
    }

    post {
        always {
            mail to: "${EMAIL_RECIPIENTS}",
                 subject: "Java Pipeline - ${currentBuild.currentResult}",
                 body: "Build result: ${currentBuild.currentResult}"
        }
    }
}
