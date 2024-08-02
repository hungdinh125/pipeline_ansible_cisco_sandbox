// Check if Jenkins server has ansible to run the playbook
pipeline {
    agent any
    stages {
        stage('Build Docker container') {
            steps {
                echo 'Build container'
                sh "docker run -dit --name ansible_git netdevops/ansible_git_v1"
            }
        }
        
        stage('Clone the repository to container') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "git clone https://github.com/hungdinh125/pipeline_ansible_cisco_sandbox.git"'                
            }
        }
                
        stage('Save running config - DEV') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook save_config.yml -i hosts_dev"'
            }
        }
        stage('Deploy New Configuration - DEV') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook conf.yml -i hosts_dev"'
            }
        }
        stage('Retrieve and validate show commands - DEV') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook verify.yml -i hosts_dev"'
            }
        }
        stage('Save running config - PROD') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook save_config.yml -i hosts_prod"'
            }
        }
        stage('Deploy New Configuration - PROD') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook conf.yml -i hosts_prod"'
            }
        }
        stage('Retrieve and validate show commands - PROD') {
            steps {
                sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook verify.yml -i hosts_prod"'
            }
        }
    }
    post {
        always {
            cleanWs(cleanWhenNotBuilt: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
            sh "docker rm -f ansible_git"
        }
    }
}
