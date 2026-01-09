pipeline{
    agent any

    environment {
        APP_NAME = 'calculator-app'
        APP_VERSION = '1.0.0'
        DOCKER_IMAGE = '${APP_NAME}'
        DOCKER_TAG = '${BUILD_NUMBER}'
    }

    options {
        buildDiscarder(logRotator(numToKeepStr: '10'))
        timeout(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
    }

    parameters {
        choice(
            name: 'ENVIRONMENT', 
            choices: ['dev', 'staging', 'prod'],
            description: 'Target deployment environment'
        )
        booleanParam(
            name: "SKIP_TESTS", 
            defaultValue: false,
            description: 'Skip test exectution'
        )
        booleanParam(
            name: 'BUILD_DOCKER',
            defaultValue: true,
            description: 'Build docker image'

        )
    }

    // tools {
    //     maven 'Maven-3.9'
    // }

    stages {
        stage('Initializing'){
            steps {
                echo '==========================================='
                echo '   Jenkins Pipeline - Calculator Demo'
                echo '==========================================='
                echo "Application: ${APP_NAME} v${APP_VERSION}"
                echo "Build Number: ${BUILD_NUMBER}"
                echo "Environment: ${params.ENVIRONMENT}"
                echo "Workspace: ${WORKSPACE}"
                echo '==========================================='
            }
        }
        stage('Checkout'){
            steps {
                echo 'Checking out source code...'
                checkout scm
                sh '''
                    echo "Current directory contents:"
                    ls -la
                    echo ""
                    echo "Java source files:"
                    find . -name "*.java" -type f 2>/dev/null || true                
                '''
            }
        }
        stage('Build') {
            steps{
            echo 'Building the application with Maven...'
            echo 'Maven version:'
            sh './mvnw -version'
            sh './mvnw clean compile'
            }
        }

        stage('Test'){
            when {
                expression {params.SKIP_TESTS == false}
            }
            steps {
                echo 'Executing JUnit tests...'
                sh './mvnw test'
            }
            post {
                always {
                    junit(
                        testResults: 'target/surefire-reports/*.xml',
                        allowEmptyResults: true
                    )
                }
                success {
                    echo 'All tests passed successfully!'
                }
                failure {
                    echo 'Some tests failed'
                }
            }
        }
    }

    post{
        always{
            echo """'==========================================='
            echo '   Pipeline Execution Complete'
            echo '==========================================='
            echo "Result: ${currentBuild.currentResult}"
            echo "Duration: ${currentBuild.durationString}"
            echo '==========================================='"""
        }
        success {
            echo 'Pipeline Succeeded! 🎉'
        }
        failure {
            echo 'Pipeline Failed! ❌'
        }
        unstable {
            echo 'Pipeline Compelted with Warnings! ⚠️'
        }
    }
}
