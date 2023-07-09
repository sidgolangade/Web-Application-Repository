pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clean workspace and checkout the code from the GitHub repository
                cleanWs()
                git 'https://github.com/sidgolangade/Web-Application-Repository.git'
            }
        }
        
        stage('Build') {
            steps {
                // Install Python and required dependencies
                sh 'pip install -r requirements.txt'
                // Any additional build steps you may have
                
                // Collect static files (if applicable)
                sh 'python manage.py collectstatic --noinput'
            }
        }

        stage('Test') {
            steps {
                // Run tests for the Django application
                sh 'python manage.py test'
            }
        }
        
        stage('Archive Artifacts') {
            steps {
                archiveArtifacts(artifacts: '**/*', excludes: '')
            }
        }
        
        stage('Deploy') {
            steps {
                // Run commands on Ansible Server using SSH
                // credentialsId: 'ansible-server-ssh-credentials' (Replace 'ansible-server-ssh-credentials' with the actual credential ID you have configured for the SSH private key in Jenkins.)
                // You can find the credential ID by going to the Jenkins credential configuration or by checking the Jenkins credential management interface.
                // Example Command: ssh -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" ansible-user@ansible-server.example.com 'ansible-playbook -i /path/to/ansible-hosts /path/to/deploy-web-app.yml'
                
                withCredentials([sshUserPrivateKey(credentialsId: '2a67236b-f587-44a4-90f2-1e8ba1e8d313', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" ec2-user@ec2-52-48-44-190.eu-west-1.compute.amazonaws.com 'ansible-playbook -i /etc/ansible/ansible-hosts /home/ec2-user/ansible-playbooks/deploy-web-app.yml'
                    '''
                }
            }
        }
    }
}

