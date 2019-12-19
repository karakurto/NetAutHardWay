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
                sh 'git pull'
		sh 'find ./Step*/  \\( -name "*.yml" -o -name "*.yaml" \\) -exec yamllint -c ./my_yamllint_config.yml {} +' 
                sh 'ansible-playbook -i hosts Step2-config/config_delivery.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/bgp_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/ping_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/vlan_test.yml'
            }
        }
        stage('Delivery') {
 	when{
            expression {
                return env.BRANCH_NAME != 'master';
            } 
	}
            steps {
		sh 'git pull origin master'
		sh 'find ./Step*/  \\( -name "*.yml" -o -name "*.yaml" \\) -exec yamllint -c ./my_yamllint_config.yml {} +' 
                sh 'ansible-playbook -i hosts Step2-config/config_delivery.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/bgp_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/ping_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/vlan_test.yml'				
            }
        }
        stage('Deployment Approval') {
 	when{
            expression {
                return env.BRANCH_NAME != 'master';
            } 
	}
            steps {
		input """
		      Are you you sure to proceed with Prod Depyloment?
		      Local branch will be pushed to the GitHub and this will trigger a deployment to the Live Network	
                """
		sh 'whoami'
		sh 'git push origin HEAD:master'
	    }
        }

        stage('Deployment') {
 	when{
            expression {
                return env.BRANCH_NAME == "NetLab_master";
            } 
	}
            steps {
		sh 'find ./Step*/  \\( -name "*.yml" -o -name "*.yaml" \\) -exec yamllint -c ./my_yamllint_config.yml {} +' 
                sh 'ansible-playbook -i hosts Step2-config/config_delivery.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/bgp_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/ping_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/integration_tests/vlan_test.yml'				
            }
        }
        stage('Post-Deployment') {
 	when{
            expression {
                return env.BRANCH_NAME == "NetLab_master";
            } 
	}
            steps {
		sh 'ansible-playbook -i hosts Step4-CICD/deployment_tests/port_status_test.yml'
		sh 'ansible-playbook -i hosts Step4-CICD/deployment_tests/dynamic_status_test.yml'				
            }
        }
    }
}
