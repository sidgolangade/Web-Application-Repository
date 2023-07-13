pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Clean workspace and checkout the code from the GitHub repository
                cleanWs()
                git branch: 'main', url: 'https://github.com/sidgolangade/Web-Application-Repository.git'
            }
        }

        stage('Build') {
            steps {
                // Install Python and required dependencies
                sh 'pip install -r requirements.txt'
                // Any additional build steps you may have

                // Collect static files (if applicable)
                //sh 'python manage.py collectstatic --noinput'
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

        stage('Deploy to Ansible Server') {
            steps {
                // Copy the built application code to the Ansible Server
                withCredentials([sshUserPrivateKey(credentialsId: '2a67236b-f587-44a4-90f2-1e8ba1e8d313', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    rsync -e "ssh -o StrictHostKeyChecking=no -i $SSH_PRIVATE_KEY" -av --exclude venv/ ./ ec2-user@ec2-54-170-68-249.eu-west-1.compute.amazonaws.com:/home/ec2-user/ansible-data/
                    '''
                }
            }
        }
                    // scp -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" -r ./* ec2-user@ec2-54-170-68-249.eu-west-1.compute.amazonaws.com:/home/ec2-user/ansible-data

        stage('Deploy using Ansible') {
            steps {
                // Run Ansible playbook on the Ansible Server
                withCredentials([sshUserPrivateKey(credentialsId: '2a67236b-f587-44a4-90f2-1e8ba1e8d313', keyFileVariable: 'SSH_PRIVATE_KEY')]) {
                    sh '''
                    ssh -o StrictHostKeyChecking=no -i "$SSH_PRIVATE_KEY" ec2-user@ec2-54-170-68-249.eu-west-1.compute.amazonaws.com 'ansible-playbook -i /etc/ansible/hosts /home/ec2-user/ansible-data/ansible-playbooks/deploy-web-app.yml'
                    '''
                }
            }
        }
    }
}

