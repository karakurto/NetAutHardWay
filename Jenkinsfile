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
<<<<<<< HEAD
                sh 'ansible-playbook -i Step2-config/hosts Step2-config/config_delivery.yml'
=======
                sh 'pwd'
>>>>>>> 36f7ceab7ba9221a70b59aa5cdc39534f3f48ea0
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
