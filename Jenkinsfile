pipeline {
    agent any

    options {
        timestamps()
    }

    environment {
        // Makes Ansible output more readable in Jenkins logs
        ANSIBLE_STDOUT_CALLBACK = 'minimal'
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Deploy/Restart Tomcat') {
            steps {
                echo "Deploying Tomcat on target servers..."
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
            echo "Tomcat deployment and verification completed successfully!"
        }
        failure {
            echo "Tomcat deployment or verification failed. Check logs for details."
        }
    }
}
