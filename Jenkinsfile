pipeline{
    agent any
	
    parameters{
	    choice(name: 'Choice_Remove', choices: ['Odd', 'Even','All'], description: 'Select App Server To Be Deployed..')
	}
	
    environment {
    vremove_odda  = servertoremove()
     }
	
    tools{
        maven 'Apache Maven 3.0.5'
        jdk 'OpenJDk'
    }
	
    stages{
        stage('git pull'){
            steps{
            //  git branch: 'main', url: 'https://github.com/singhakumar/wallets.git' 
                git branch: params.DynamicBranches, url: 'https://github.com/singhakumar/wallets.git'
            }
        }
        stage('initialize'){
            steps{
                   sh '''
                   echo "PATH = ${PATH}"
                   echo "M2_HOME = ${M2_HOME}"
                  '''
            }
        }
        stage ('Build project'){
            steps{
                    sh 'mvn clean'
                    sh 'mvn clean package'
             //     sh 'mvn archetype:generate -DgroupId = in.xo -DartifactId = wallets -DarchetypeArtifactId = maven-archetype-webapp -DinteractiveMode = false'
               
            }
        }
        
        
        
        stage('Remove Upstream'){
            steps{
                ansiblePlaybook become: true, extras: "-e ip_address=${vremove}", installation: 'ansible 2.9.15', inventory: '/var/lib/jenkins/ansible-playbooks/inve', playbook: '/var/lib/jenkins/ansible-playbooks/upstream_down_job.yaml'
             //   echo "Upstream to be removed ${vremove}"
            }
        }
      
        stage('Approval To Continue') {
            steps {
                input('Do you want to proceed?')
            }
        }

        // War Deployment Stage         
        stage('War Deployment Stage') {
            steps {
		sleep(time:30,unit:"SECONDS")
                ansiblePlaybook become: true, extras: "-e app_var=${vremove_odda}", installation: 'ansible 2.9.15', inventory: '/var/lib/jenkins/ansible-playbooks/inve', playbook: '/var/lib/jenkins/ansible-playbooks/upstream_deploy.yaml'
            }
        }

      
         stage('Add Upstream'){
            steps{
                ansiblePlaybook become: true, extras: "-e ip_address=${vremove}", installation: 'ansible 2.9.15', inventory: '/var/lib/jenkins/ansible-playbooks/inve', 
playbook: '/var/lib/jenkins/ansible-playbooks/upstream_up_job.yaml'
            }
        }
        
    }
}


def servertoremove(){
	 if(params.Choice_Remove == 'Odd')
	 {
        vremove='192.168.20.181,192.168.20.183'
        vremove_odd='oddapp' 
		return vremove_odd
     }   
   
   if(params.Choice_Remove == 'Even')
   {
        vremove='192.168.20.182' 
        vremove_odd='evenapp'
        return vremove_odd		
   }   
}
