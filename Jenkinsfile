pipeline{
    agent any

    environment {
        APP_NAME = 'calculator-app'
        APP_VERSION = '1.0.0'
        DOCKER_IMAGE = '${APP_NAME}'
        DOCKER_TAG = '${BUILD_NUMBER}'
    }

    options {
        buildDisarder(logRotator(numToKeepStr: '10'))
        timput(time: 30, unit: 'MINUTES')
        timestamps()
        disableConcurrentBuilds()
    }

    parameters {
        choice(
            name: 'ENVIRONMENT', 
            choice: ['dev', 'staging', 'prod'],
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
    }

    post{
        always{
            echo '==========================================='
            echo '   Pipeline Execution Complete'
            echo '==========================================='
            echo "Result: ${currentBuild.currentResult}"
            echo "Duration: ${currentBuild.durationString}"
            echo '==========================================='
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
