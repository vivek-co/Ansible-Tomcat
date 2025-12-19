pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        // Make Ansible output cleaner in Jenkins logs
        ANSIBLE_STDOUT_CALLBACK = 'minimal'
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
                    extras: '--ssh-common-args="-o StrictHostKeyChecking=no"'
                )
            }
        }

        stage('Verify Tomcat') {
            steps {
                echo "Verifying Tomcat status on target servers..."
                ansiblePlaybook(
                    playbook: 'verify_tomcat.yml',
                    inventory: 'hosts.ini',
                    extras: '--ssh-common-args="-o StrictHostKeyChecking=no"'
                )
            }
        }
    }

    post {
        success {
            echo "Tomcat deployment completed successfully!"
        }
        failure {
            echo "Tomcat deployment failed. Check logs for details."
        }
    }
}
