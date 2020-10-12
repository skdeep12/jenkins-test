def remote = [:]
remote.name = "staging-bastion"
remote.host = "bastion.dehaatagri.com"
remote.allowAnyHosts = true
remote.port = 272

def kheti_prod = [:]
kheti_prod.name = "kheti-prod"
kheti_prod.host = "172.31.26.59"
kheti_prod.allowAnyHosts = true
kheti_prod.port = 22

def celery_prod = [:]
celery_prod.name = "celery-prod"
celery_prod.host = "172.31.23.47"
celery_prod.allowAnyHosts = true
celery_prod.port = 22

pipeline {
    agent any
    parameters {
        string(defaultValue: "$BUILD_NUMBER", description: 'What is the build number?', name: 'APP_BUILD_VERSION')
    }
    stages {
        stage("ssh step"){
            steps {
                withCredentials([
                    [$class: 'SSHUserPrivateKeyBinding', credentialsId: 'prod-kheti', keyFileVariable: 'kheti_prod_key',passphraseVariable: '',usernameVariable: 'kheti_user']
                ]) {
                    script {
                        kheti_prod.user = kheti_user
                        kheti_prod.identityFile = kheti_prod_key
                    }
                    sshCommand remote: kheti_prod,command: "cd kheti && eval `ssh-agent` && ssh-add ~/.ssh/github_rsa && git pull"
                    sshCommand remote: kheti_prod,command: "cd kheti && source venv/bin/activate && python manage.py migrate"
                    sshCommand remote: kheti_prod, sudo: true, command: "systemctl stop kheti" 
                    sshCommand remote: kheti_prod, sudo: true, command: "systemctl start kheti"
                    sshCommand remote: kheti_prod, sudo: true, command: "systemctl status kheti"
                }
            }
        }
    }
}
