pipeline{
    agent any
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
        
        stage('Get Upstream To Removed'){
         steps{
            script{
                if(params.Choice_Remove == 'Odd'){
                 stage('Odd Removed'){
                      vremove='192.168.20.181,192.168.20.183'  
                 }   
                }
                if(params.Choice_Remove == 'Even'){
                 stage('Even Removed'){
                     vremove='192.168.20.182'  
                 }   
                }
            }
             echo vremove
           }
        }
        
        stage('Down Upstream'){
            steps{
                echo "Upstream to be removed ${vremove}"
            }
        }
    }
}
