pipeline {
    agent any

    parameters {
        choice(name: 'ENV', choices: ['dev', 'qa', 'prod'], description: 'Select environment')
    }

    environment {
        APP_NAME = "my-test-app"
        VERSION = "1.0.${BUILD_NUMBER}"
    }

    stages {

        stage('Start') {
            steps {
                echo "Hello Jenkins! Starting pipeline for ${APP_NAME} version ${VERSION}"
            }
        }

        stage('Build') {
            steps {
                echo "Simulating build for environment: ${params.ENV}"
                sh 'echo "Build completed!"'
            }
        }

        stage('Test') {
            steps {
                echo "Simulating tests..."
                sh 'echo "Tests passed!"'
            }
        }

        stage('Deploy') {
            steps {
                echo "Deploying ${APP_NAME} to environment: ${params.ENV}"
                sh 'echo "Deployment simulated!"'
            }
        }
    }

    post {
        success {
            echo "Pipeline ran successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
