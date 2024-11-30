pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SudheerKumarP0357/multi-tier-java-mysql.git'
            }
        }

        stage('Neccessary Permissions') {
            steps {
              sh 'chmod +x ./mvnw'
            }
        }

        stage('Build') {
            steps {
              sh './mvnw clean package'
            }
        }
        stage('copy the jar file to home location') {
            steps {
              sh 'cp target/*.jar /home/sudheerkumarp0357'
            }
        }
      
    }
}
