pipeline {
    agent any

    parameters {
        booleanParam(
            name: 'UPDATE_marques512sv',
            defaultValue: true,
            description: 'UPDATE marques512sv.machine'
        )
        booleanParam(
            name: 'UPDATE_marques2673sv',
            defaultValue: true,
            description: 'UPDATE marques2673sv.machine'
        )
        booleanParam(
            name: 'UPDATE_vm_tools',
            defaultValue: true,
            description: 'UPDATE vm.tools'
        )
        booleanParam(
            name: 'UPDATE_vm_commov',
            defaultValue: true,
            description: 'UPDATE vm.commov'
        )
        booleanParam(
            name: 'UPDATE_vm_immich',
            defaultValue: true,
            description: 'UPDATE vm.immich'
        )
        booleanParam(
            name: 'UPDATE_vm_minelive',
            defaultValue: true,
            description: 'UPDATE vm.minelive'
        )
        booleanParam(
            name: 'UPDATE_vm_onlineprojects',
            defaultValue: true,
            description: 'UPDATE vm.onlineprojects'
        )
        booleanParam(
            name: 'UPDATE_vm_vault',
            defaultValue: true,
            description: 'UPDATE vm.vault'
        )
    }

    stages {
        stage('Check Ansible') {
            steps {
                sh 'ansible --version'
            }
        }

        stage('Update Machines') {
            steps {
                script {
                    def machines = [
                        [param: 'UPDATE_marques512sv', host: 'marques512sv.machine'],
                        [param: 'UPDATE_marques2673sv', host: 'marques2673sv.machine'],
                        [param: 'UPDATE_vm_tools', host: 'vm.tools'],
                        [param: 'UPDATE_vm_commov', host: 'vm.commov'],
                        [param: 'UPDATE_vm_immich', host: 'vm.immich'],
                        [param: 'UPDATE_vm_minelive', host: 'vm.minelive'],
                        [param: 'UPDATE_vm_onlineprojects', host: 'vm.onlineprojects'],
                        [param: 'UPDATE_vm_vault', host: 'vm.vault']
                    ]

                    def selectedMachines = machines.findAll { params[it.param] }

                    if (selectedMachines.isEmpty()) {
                        echo 'No machines selected.'
                        return
                    }

                    withCredentials([usernamePassword(credentialsId: 'EXAMPLE_SSH_CREDENTIAL_ID', usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASSWORD')]) {
                        selectedMachines.each { machine ->
                            sh """
                                ANSIBLE_HOST_KEY_CHECKING=False ansible-playbook \
                                  -i ansible/inventory/machines.ini \
                                  --limit '${machine.host}' \
                                  --extra-vars "ansible_password=\${SSH_PASSWORD} ansible_become_password=\${SSH_PASSWORD}" \
                                  ansible/playbooks/update-fedora.yml
                            """
                        }
                    }
                }
            }
        }
    }
}
