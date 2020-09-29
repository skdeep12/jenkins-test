
pipeline {
    agent any
    parameters {
        string(defaultValue: "$BUILD_NUMBER", description: 'What is the build number?', name: 'APP_BUILD_VERSION')
    }
    stages {
        stage('Build') {
            steps {
                echo "build number is something, just checking trigger"
                sh "ls"
            }
        }
    }
}
