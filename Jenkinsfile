
pipeline {
    agent any
    parameters {
        string(defaultValue: "$BUILD_NUMBER", description: 'What is the build number?', name: 'APP_BUILD_VERSION')
    }
    stages {
        stage("ssh step"){
            steps {
                withCredentials([[
                    $class: 'SSHUserPrivateKeyBinding',
                    credentialsId: 'staging-bastion',
                    keyFileVariable: 'identity',
                    passphraseVariable: '',
                    usernameVariable: 'user'
                ]]) {
                    def remote = [:]
                    remote.name = "node-1"
                    remote.host = "bastion.dehaatagri.com"
                    remote.allowAnyHosts = true
                    remote.port = 272
                    remote.userName = user
                    remote.identity = identity
                    sshCommand remote: remote,command: 'ls'
                }
        }
    }
}
