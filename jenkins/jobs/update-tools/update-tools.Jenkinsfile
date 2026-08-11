pipeline {
    agent any

    stages {
        stage('Check Ansible') {
            steps {
                sh 'ansible --version'
            }
        }

        stage('Update Fedora') {
            steps {
                withCredentials([usernamePassword(credentialsId: '9f0dba2d-aff5-4b30-9c8a-0a8c35ab3da2', usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASSWORD')]) {
                    sh '''
                        ansible-playbook \
                          -i ansible/inventory/fedora.ini \
                          --extra-vars "ansible_user=${SSH_USER} ansible_password=${SSH_PASSWORD} ansible_become_password=${SSH_PASSWORD}" \
                          ansible/playbooks/update-fedora.yml
                    '''
                }
            }
        }
    }
}
