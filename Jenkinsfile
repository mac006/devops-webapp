pipeline {
    agent any

    triggers {
        pollSCM('H/1 * * * *')
    }

    environment {
        ANSIBLE_CONFIG = "${WORKSPACE}/ansible/ansible.cfg"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Verify Tools') {
            steps {
                sh '''
                    set -eux

                    ansible --version
                    ansible-playbook --version
                '''
            }
        }

        stage('Inventory Check') {
            steps {
                sh '''
                    set -eux

                    cd ansible
                    ansible-inventory --graph
                '''
            }
        }

        stage('Connectivity Test') {
            steps {
                sh '''
                    set -eux

                    cd ansible
                    ansible all -m ping
                '''
            }
        }

        stage('Syntax Check') {
            steps {
                sh '''
                    set -eux

                    cd ansible
                    ansible-playbook --syntax-check site.yml
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    set -eux

                    cd ansible
                    ansible-playbook site.yml
                '''
            }
        }

        stage('Verify Deployment') {
            steps {
                sh '''
                    set -eux

                    curl -f http://192.168.0.200/
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }

        failure {
            echo 'CI/CD pipeline FAILED.'
        }

        always {
            echo 'Pipeline finished.'
        }
    }
}


