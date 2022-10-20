

pipeline {
  agent { 
  label 'ansible'
  }
  environment {
   AWS_EC2_PRIVATE_KEY=credentials('ec2-private-key') 
  }
  
  stages {
    
    //Get the Code from GitHub Repo
    stage('CheckOutCode'){
      steps{
        git branch: 'master', credentialsId: '4e16d88c-4dfc-43c9-9452-379540e48bc6', url: 'https://github.com/prashanthdevopsengineer/jenkins-with-ansible.git'
      }
    }
     
    //Using Terrafrom can create the Servers
    
    //stage('CreateServers'){
     // steps{
      // sh "terraform  -chdir=terraformscripts init"
       //sh "terraform  -chdir=terraformscripts apply --auto-approve"
    //  }
   // }
    
    //Run the playbook
    stage('RunPlaybook') {
      steps {
        sh "whoami"
        //List the dymaic inventory just for verification
        sh "ansible-inventory --graph -i inventory/aws_ec2.yaml"
        //Run playbook using dynamic inventory & limit exuection only fo tomcatservers.
        sh "ansible-playbook -i inventory/aws_ec2.yaml  playbooks/tomcat-setup.yaml -u ec2-user --private-key=$AWS_EC2_PRIVATE_KEY --limit tomcatservers --ssh-common-args='-o StrictHostKeyChecking=no'"
      }
    }
  
  }//stages closing
}//pipeline closing
