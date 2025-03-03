pipeline {
    agent any

    environment {
        GRADLE_USER_HOME = "${WORKSPACE}/.gradle" // Caching Gradle
    }

    stages {
        stage('Checkout') {
            steps {
                script {
                    def branchName = env.BRANCH_NAME
                    echo "Running on branch: ${branchName}"

                    if (!(branchName == 'main' || branchName.contains('feature'))) {
                        error("Pipeline failed: Branch not allowed.")
                    }
                }
            }
        }

        stage('Build') {
            steps {
                sh './gradlew clean build'
            }
        }

        stage('Run Tests') {
            steps {
                script {
                    if (env.BRANCH_NAME == 'main') {
                        sh './gradlew test codeCoverageReport' // Runs all tests + CodeCoverage
                    } else if (env.BRANCH_NAME.contains('feature')) {
                        sh './gradlew test' // Runs tests without CodeCoverage
                    }
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline Execution Finished'
        }
    }
}
