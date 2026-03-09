pipeline {
    agent any

    stages {

        stage("Transfer Deployment Playbook") {
            steps {
                sh '''
                set -e
                echo "Copying playbook to ansible user..."

                sudo cp $WORKSPACE/deploy-container.yml /home/ansible/
                sudo chown ansible:ansible /home/ansible/deploy-container.yml

                echo "Playbook copied!"
                '''
            }
        }

        stage("Run Ansible Playbook") {
            steps {
                sh '''
                set -e
                echo "Executing Ansible playbook..."

                sudo -u ansible ansible-playbook /home/ansible/deploy-container.yml

                echo "Deployment finished!"
                '''
            }
        }

    }
}