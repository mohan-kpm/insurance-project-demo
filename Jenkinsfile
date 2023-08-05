node{
        stage('GIT Checkout'){
            echo 'Check out application code from GIT'
            git 'https://github.com/mohan-kpm/insurance-project-demo.git'
        }
        
        stage('maven build'){
            sh 'mvn clean package'
        }
        
        stage('Build Docker image'){
            sh 'sudo docker build -t mohandocker8/insure-me:3.0 .'
        }
        
        stage('Push docker image to docker hub registry')
        {
            echo 'Pushing images to docker registry'
            
            withCredentials([string(credentialsId: 'dockerHubPwd', variable: 'dockerHubPassword')]) {
                sh "sudo docker login -u mohandocker8 -p ${dockerHubPassword}"
                sh 'sudo docker push mohandocker8/insure-me:3.0'
            }
        }
        
        stage('configure test-server and deploy insure-me'){
            echo "configuring test-server"
          //  sh 'ansible-playbook configure-test-server.yml'
            ansiblePlaybook become: true, credentialsId: 'ansible-user', disableHostKeyChecking: true, installation: 'Ansible', inventory: '/etc/ansible/hosts', playbook: 'configure-test-server.yml'
            
        }
        
}
