pipeline {
    agent any

    tools {
        // Specify the Maven installation configured in Jenkins
        maven 'MavenInstallation'
    }

    stages {
        stage('Checkout') {
            steps {
                // Clone the repository
                git 'https://github.com/sidgolangade/Web-Application-Repository.git'
            }
        }

        stage('Build') {
            steps {
                // Run Maven build
                sh 'mvn clean package'
            }
        }
    }
}

