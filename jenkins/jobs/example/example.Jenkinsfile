pipeline {
    agent any

    stages {
        stage('Check Ansible') {
            steps {
                sh 'ansible --version'
            }
        }

        stage('Run Playbook') {
            steps {
                sh 'ansible-playbook ansible/example/playbook.yml'
            }
        }
    }
}
