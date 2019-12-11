pipeline {
    agent any
    stages {
        stage('Integration') {
 	when{
            expression {
                return env.BRANCH_NAME != 'master';
            } 
	}
            steps {
                sh 'ansible-playbook -i Step2-config/hosts Step2-config/config_delivery.yml'
            }
        }
        stage('Delivery') {
 	when{
            expression {
                return env.BRANCH_NAME != 'master';
            } 
	}
            steps {
                sh 'whoami'				
            }
        }
        
    }
}
