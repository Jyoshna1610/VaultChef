pipeline {
environment {
        
        AWS_DEFAULT_REGION = "us-east-1"
    }
agent  any
stages {
         stage('Vault - AWS connection check') {
            steps {
                withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                       sh '''
                        aws --version
                        aws ec2 describe-instances
                        '''
}
            }
        }
        stage('checkout') {
            steps {
                 script{

                        
                           git "https://github.com/Jyoshna1610/VaultChef.git"
                        
                    }
                }
            }
                            
        stage('Plan') {
            steps {
                     withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                bat 'cd&cd terraform/Terraform-Vault & terraform init -input=false'
                bat 'cd&cd terraform/Terraform-Vault & terraform destroy -auto-approve'
                bat "cd&cd terraform/Terraform-Vault & terraform plan -input=false -out tfplan"
                bat 'cd&cd terraform/Terraform-Vault & terraform show -no-color tfplan > tfplan.txt'
                     }
            }
        }
       

        stage('Apply') {
            steps {
                    withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
                bat "cd&cd terraform/Terraform-Vault & terraform apply -input=false tfplan"
                    }
            }
        }
        
        /*stage('Slack Notification'){
               steps{
         slackSend message:"Team DevOps:Please approve ${env.JOB NAME} ${env.BUILD_NUMBER} (<$env.JOB_URL}|Open>)"
                }
              }
        stage('Let the human feel important'){
              input { message "Click Proceed to continue the build"}
                steps {
                echo "User Input"
                }
        }
        stage('deployment'){
                steps{
       /*
         withCredentials([zip(credentialsID:'chef-server-creds',variable:'CHEFREPO')]){
         sh'mkdir-p $CHEFREPO/chef-repo/cookbooks/apache'
         sh 'sudo rm -rf $WORKSPACE/Berksfile.lock'
         sh 'mv $WORKSPACE/* $CHEFREPO/chef-repo/cookbooks/apache'
         sh "knife cookbook upload apache --force -o $CHEFREPO/chef-repo/cookbooks -c $CHEFREPO/chef-repo/.chef/knife.rb"
     
         withCredentials([vaultString(credentialsId: 'AWS_ACCESS_KEY_VAULT', variable: 'AWS_ACCESS_KEY_ID'), vaultString(credentialsId: 'AWS_SECRET_ACCESS_KEY_VAULT', variable: 'AWS_SECRET_ACCESS_KEY')]) {
         sh "knife ssh 'role:webserver' -x ec2-user -i $TERRA-CHEF-JEN 'sudo Chef-client' -c $CHEFREPO/chef-repo/.chef/knife.rb"
         }
      }*/
         
     }
   

