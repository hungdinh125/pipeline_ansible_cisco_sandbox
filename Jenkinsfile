// Check if Jenkins server has ansible to run the playbook
pipeline {
    agent any
    stages {
        stage('Clone the repository') {
            steps {
                git (branch: 'main',
                     url: 'https://github.com/hungdinh125/jenkins.git')
            }
        }
        stage('Deploy New Configuration - DEV') {
            steps {
                sh "ansible-playbook conf.yml -i hosts_dev"
            }
        }
        stage('Retrieve and validate show commands - DEV') {
            stepss {
                sh "ansible-playbook verify.yml -i hosts_dev"
            }
        }
        stage('Deploy New Configuration - PROD') {
            steps {
                sh "ansible-playbook conf.yml -i hosts_prod"
            }
        }
        stage('Retrieve and validate show commands - PROD') {
            stepss {
                sh "ansible-playbook verify.yml -i hosts_prod"
            }
        }
    }
    post {
        always {
            cleanWs(cleanWhenNotBuild: false,
                    deleteDirs: true,
                    disableDeferredWipeout: true,
                    notFailBuild: true)
        }
    }
}