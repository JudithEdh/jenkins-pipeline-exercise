pipeline{
        agent any
        stages{
            stage('Make Directory'){
                steps{
                  sh '''
                  #! /bin/bash
                  DIRECTORY=~/jenkins-pipeline-exercise  
                  rm -rf DIRECTORY
                  if [ -d ~/jenkins-pipeline-exercise ]
                  then
                      rm -rf $DIRECTORY
                  else 
                    mkdir $DIRECTORY
                    cd $DIRECTORY
                  fi          
                  '''
                }
            }        
            stage('Clone Repo if it does not exist'){
                steps{
                  sh '''
                  DIRECTORY=~/jenkins-pipeline-exercise  
                  FILE=/home/jenkins/.jenkins/workspace/jenkins-pipeline-exercise/chaperootodo_client
                  sudo apt-get install git                
                  if [ -d "$FILE" ]
                  then
                    echo exists
                  else 
                    git clone https://gitlab.com/qacdevops/chaperootodo_client 
                  fi 
                  cd $FILE
                  '''
                }
            }
            stage('Install Docker and Docker-Compose'){
                steps{
                  sh '''
                  sudo apt update
                  curl https://get.docker.com | sudo bash
                  sudo usermod -aG docker $(whoami)
                  sudo apt install -y curl jq
                  version=$(curl -s https://api.github.com/repos/docker/compose/releases/latest | jq -r '.tag_name')
                  sudo curl -L "https://github.com/docker/compose/releases/download/${version}/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
                  sudo chmod +x /usr/local/bin/docker-compose
                  '''
                }
              }
            stage('Deploy the application'){
                steps{
                    sh '''
                    export DB_PASSWORD
                    cd /home/jenkins/.jenkins/workspace/jenkins-pipeline-exercise/chaperootodo_client
                    DB_PASSWORD=password
                    export DB_PASSWORD && sudo docker-compose pull && sudo docker-compose up -d
                    '''  
                }
            }
        }    
}
