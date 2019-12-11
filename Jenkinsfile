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
                sh 'pwd'
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
