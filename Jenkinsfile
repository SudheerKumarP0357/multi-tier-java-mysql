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

        stage('Run the application') {
            steps {
                sh 'nohup java -jar /home/sudheerkumarp0357/bankapp-0.0.1-SNAPSHOT.jar --server.port=8081 > app.log 2>&1 &'
                sh 'echo "Access the application: http://$(wget -qO- ifconfig.me):8081/login"'
            }
        }
      
    }
}
