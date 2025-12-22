pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "yourdockerhubuser/devops-app"
        VERSION = "1.0.${BUILD_NUMBER}"
    }

    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
    }

    stages {

        stage('Checkout Code') {
            steps {
                git'https://github.com/ubaid-syed016/jenkins-learning'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'pip install -r app/requirements.txt'
            }
        }

        stage('Unit Tests') {
            steps {
                sh 'pytest tests/unit/'
            }
        }

        stage('Integration Tests') {
            steps {
                sh 'pytest tests/integration/'
            }
        }

        stage('Lint / Code Quality') {
            steps {
                echo "Linting step (can add flake8 later)"
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t $DOCKER_IMAGE:$VERSION ."
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerhub-creds',
                    usernameVariable: 'USER',
                    passwordVariable: 'PASS'
                )]) {
                    sh """
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push $DOCKER_IMAGE:$VERSION
                    """
                }
            }
        }

        stage('Deploy') {
            when {
                expression { params.DEPLOY_ENV != null }
            }
            steps {
                sh "docker run -d --env-file=config/${DEPLOY_ENV}.env -p 5000:5000 $DOCKER_IMAGE:$VERSION"
            }
        }
    }

    post {
        success {
            echo "Pipeline Successful!"
        }
        failure {
            echo "Pipeline Failed!"
        }
    }
}

