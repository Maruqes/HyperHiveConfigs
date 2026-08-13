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
                        [param: 'UPDATE_marques512sv', host: 'marques512sv.machine', credentialsId: 'af27978f-ee69-494c-a4e5-3f63162ce8fc'],
                        [param: 'UPDATE_marques2673sv', host: 'marques2673sv.machine', credentialsId: 'd28eafc4-23e5-431d-bd07-1b921e2b5616'],
                        [param: 'UPDATE_vm_tools', host: 'vm.tools', credentialsId: '9f0dba2d-aff5-4b30-9c8a-0a8c35ab3da2'],
                        [param: 'UPDATE_vm_commov', host: 'vm.commov', credentialsId: '5ad1246b-d360-4e36-a3c6-bca50183ed16'],
                        [param: 'UPDATE_vm_immich', host: 'vm.immich', credentialsId: '7b20f729-b5f2-44c0-b4f3-4c49a8fba37e'],
                        [param: 'UPDATE_vm_minelive', host: 'vm.minelive', credentialsId: '453618e7-70d9-4ea8-ad43-d951bde6eeab'],
                        [param: 'UPDATE_vm_onlineprojects', host: 'vm.onlineprojects', credentialsId: '8e709aa4-7bbc-4e0a-a81f-870c5cd33655'],
                        [param: 'UPDATE_vm_vault', host: 'vm.vault', credentialsId: '27187756-b433-452d-b83a-a62c7784cc33']
                    ]

                    def selectedMachines = machines.findAll { params[it.param] }

                    if (selectedMachines.isEmpty()) {
                        echo 'No machines selected.'
                        return
                    }

                    selectedMachines.each { machine ->
                        withCredentials([usernamePassword(credentialsId: machine.credentialsId, usernameVariable: 'SSH_USER', passwordVariable: 'SSH_PASSWORD')]) {
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
