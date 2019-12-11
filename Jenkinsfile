pipeline {
    agent any
    when{
         expression {
            return env.BRANCH_NAME != 'master';
            } 
	 }
    stages {
        stage('Integration') {
            steps {
                sh 'pwd'
            }
        }
        stage('Delivery') {
            steps {
                sh 'whoami'				
            }
        }
        
    }
}
