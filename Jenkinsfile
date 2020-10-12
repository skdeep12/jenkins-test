def remote = [:]
remote.name = "staging-bastion"
remote.host = "bastion.dehaatagri.com"
remote.allowAnyHosts = true
remote.port = 272
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
                    script {
                        remote.user = user
                        remote.identity = identity
                    }
                    sshCommand remote: remote,command: 'ls'
                }
            }
        }
    }
}
