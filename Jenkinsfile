pipeline {
    agent any
    stages {
       stage('gitlab') {
          steps {
             echo 'Notify GitLab'
             updateGitlabCommitStatus name: 'build', state: 'pending'
             updateGitlabCommitStatus name: 'build', state: 'success'
          }
       }
       stage('gitClone'){
          steps{
                  git url: 'http://54.254.157.242/devops-2023-01/tung-project.git', branch: 'main', credentialsId: 'gitlab'
          }
       }
       stage('buid image'){
         steps{
            dir("/var/lib/jenkins/workspace/demo"){
               sh ' docker build -t tungnguyenhcl/nginx:latest .'
            }
         }
       } 
       stage ('push'){
          steps{
             withDockerRegistry(credentialsId: 'DOCKERHUB', url: 'https://index.docker.io/v1/') {
                sh 'docker push tungnguyenhcl/nginx'
               }
            }
       }
       stage ('execute ansible'){
          steps{
             ansiblePlaybook (credentialsId:'jenkinsAnsible', installation: 'Ansible', inventory: 'inventory', playbook: 'playbook.yaml')
          }
       }       
   }
}
