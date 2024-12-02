pipeline {
    agent any

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SudheerKumarP0357/multi-tier-java-mysql.git'
            }
        }
      
        stage('Build') {
            steps {
              sh 'mvn clean package'
            }
        }   
    }
}
