pipeline{
        agent any
        stages{
            stage('Make Directory'){
                steps{
                  sh '''
                  #! /bin/bash
                  DIRECTORY=~/jenkins-pipeline-exercise                 
                  if [ -d ~/jenkins-pipeline-exercise ]
                  then
                    cd $DIRECTORY
                  else 
                    mkdir DIRECTORY
                    cd $DIRECTORY
                  fi          
                  '''
                }
            }        
            stage('Clone Repo if it does not exist'){
                steps{
                  sh '''
                  DIRECTORY=~/jenkins-pipeline-exercise  
                  FILE=~/qacdevops/chaperootodo_client
                  sudo apt-get install git
                  cd $DIRECTORY
                  if [ -d "$FILE" ]
                  then
                    cd $FILE && echo exists
                  else 
                    cd $DIRECTORY && git clone https://gitlab.com/qacdevops/chaperootodo_client && cd $FILE
                  fi          
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
                    sh "sudo docker-compose up -d"
                }
            }
        }    
}
