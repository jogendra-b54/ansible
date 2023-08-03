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
            steps{
                sh "env"
                sh "echo Code Scan Completed"
            }
        }
       
         stage('Ansible Lint Checks') {	
            when { branch pattern: "feature-.*", comparator: "REGEXP"}	
            steps {
                sh  "env"
                sh  "echo Running against the feature branch whose name is ${GIT_BRANCH}"		
                sh  "echo Lint Checks Completed"	
            }	
        }
        stage('Ansible Dry Run'){
           when { branch pattern: "PR-.*", comparator: "REGEXP"}	
            steps{
                sh '''
                
                ansible-playbook robot-dryrun.yaml -e COMPONENT=${COMPONENT} -e ansible_user=centos -e ansible_password=${SSHCRED_PSW} -e ENV=dev 

                '''
            }
        }

        // stage('Promoting Code to PROD Branch'){
        //     when {
        //         branch 'main'
        //     }
        //     steps{
        //         sh "echo merging the featues branch to PROD Branch"
        //     }
        // }
        stage('Promoting Code to PROD Branch'){
            when {  expression { env.TAG_NAME == ".*" }} // TAG_NAME Env variable will only be available if you run it against the TAG 
            steps {
                sh "echo merging the featues branch to PROD Branch"
            }
        }
    }
}