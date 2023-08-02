pipeline {
    agent any   
    environment {
        SSHCRED =  credentials('SSH_CRED')
    }
    parameters {
         string(name: 'COMPONENT', defaultValue: 'mongodb', description: 'Enter the name of the component')
    }
    stages {
        stage('Ansible Code Scan'){
            steps {
                sh "echo Code Scan Completed"
            }
        }
        

        stage('Ansible Dry Run'){
            steps{
                sh '''
                
                ansible-playbook robot-dryrun.yaml -e COMPONENT=${COMPONENT} -e ansible_user=centos -e ansible_password=${SSHCRED_PSW} -e ENV=dev 

                '''
            }
        }

        stage('Promoting Code to PROD Branch'){
            steps{
                sh "echo merging the featues branch to PROD Branch"
            }
        }

    }
}