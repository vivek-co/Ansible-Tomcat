pipeline {
    agent any

    options {
        timestamps()
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy Tomcat using Ansible') {
            steps {
                ansiblePlaybook(
                    playbook: 'playbook.yml',
                    inventory: 'inventory/hosts.ini',
                    extras: '-v'
                )
            }
        }
    }
}

