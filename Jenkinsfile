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
                    playbook: 'install_tomcat.yml',
                    inventory: 'hosts.ini',
                    extras: '-v --ssh-common-args="-o StrictHostKeyChecking=no"'
                )
            }
        }
    }
}
