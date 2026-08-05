pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Ansible Version') {
            steps {
                sh 'ansible --version'
            }
        }

        stage('Syntax Check') {
            steps {
                sh '''
                    if [ -f ansible/site.yml ]; then
                        ansible-playbook --syntax-check ansible/site.yml
                    elif [ -f site.yml ]; then
                        ansible-playbook --syntax-check site.yml
                    else
                        echo "No Ansible playbook found"
                        exit 1
                    fi
                '''
            }
        }

        stage('Ansible Lint') {
            steps {
                sh '''
                    if command -v ansible-lint >/dev/null 2>&1; then
                        ansible-lint ansible/site.yml 2>/dev/null || \
                        ansible-lint site.yml
                    else
                        echo "ansible-lint not installed; skipping"
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    if [ -f ansible/site.yml ]; then
                        ansible-playbook ansible/site.yml
                    elif [ -f site.yml ]; then
                        ansible-playbook site.yml
                    else
                        echo "No Ansible playbook found"
                        exit 1
                    fi
                '''
            }
        }
    }

    post {
        success {
            echo 'CI/CD pipeline completed successfully.'
        }
        failure {
            echo 'CI/CD pipeline failed.'
        }
    }
}
