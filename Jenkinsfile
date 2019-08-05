void setBuildStatus(String message, String state) {
  step([
      $class: "GitHubCommitStatusSetter",
      reposSource: [$class: "ManuallyEnteredRepositorySource", url: env.GIT_URL],
      contextSource: [$class: "ManuallyEnteredCommitContextSource", context: "ci/jenkins/build-status"],
      errorHandlers: [[$class: "ChangingBuildStatusErrorHandler", result: "UNSTABLE"]],
      statusResultSource: [ $class: "ConditionalStatusResultSource", results: [[$class: "AnyBuildResult", message: message, state: state]] ]
  ]);
}

pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                setBuildStatus("Building...", "PENDING");
                echo 'Building...'
                sh 'platformio run'  // compile only
            }
        }
        stage('Upload') {
            steps {
                setBuildStatus("Flashing firmware...", "PENDING");
                echo 'Flashing firmware....'
                sh 'platformio run -t upload'  // compile and upload to device
            }
        }
        stage('Test') {
            steps {
                setBuildStatus("Testing...", "PENDING");
                echo 'Testing...'
                sh 'platformio test'
            }
        }
        stage('Deploy') {
            steps {
                setBuildStatus("Deploying...", "PENDING");
                echo 'Deploying....'
            }
        }
    }
    post {
        success {
            setBuildStatus("Build succeeded", "SUCCESS");
        }
        failure {
            setBuildStatus("Build failed", "FAILURE");
        }
    }
}
