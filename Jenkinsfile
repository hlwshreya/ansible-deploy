pipeline {
    agent any
    tools {
        ansible 'Ansible-2.16'
    }
    triggers {
        pollSCM('H/5 * * * *')  // Poll every 5 minutes
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Deploy Python App') {
            when {
                branch 'main'
            }
            steps {
                ansiblePlaybook(
                    playbook: 'deploy-app.yml',
                    inventory: 'inventory.ini',
                    credentialsId: 'deploy-ssh-key',
                    extras: '--become'
                )
            }
        }
    }
    post {
        always {
            ansiblePlaybook(
                playbook: 'deploy-app.yml',
                inventory: 'inventory.ini',
                credentialsId: 'deploy-ssh-key',
                extras: '--check --diff'
            )
        }
    }
}
