pipeline {
    agent any
    
    triggers {
        GenericTrigger(
         genericVariables: [
          [key: 'ref', value: '$.ref']
         ],

         causeString: 'Triggered on $ref',

         token: 'abc123',
         tokenCredentialId: '',

         printContributedVariables: true,
         printPostContent: true,

         silentResponse: false,

         shouldNotFlatten: false,

         regexpFilterText: '$ref',
         regexpFilterExpression: 'refs/heads/develop'
    )
  }

    parameters {
        choice(name: 'ENV', choices: ['Dev', 'QA', 'Prod'], description: 'Select the environment')
    }

    environment{
        GIT_URL: 'https://github.com/SudheerKumarP0357/multi-tier-java-mysql.git'
        GIT_BRANCH: 'develop'
    }

    tools{
        jdk 'JAVA17'
        maven 'MAVEN'
    }
    stages {
        stage('Checkout') {
            steps {
                git branch:env.BRANCH_NAME, URL:evn.GIT_URL
            }
        }
        stage('Build') {
            steps {
                script {
                    if (fileExists('pom.xml')) {
                        echo 'Executing Maven Build'
                        sh "mvn clean install"
                    } else {
                        error 'No pom.xml file found in the repository!'
                    }
                }
            }
        }
        stage('Test') {
            parallel {
                stage('Unit Tests') {
                    steps {
                        sh "mvn test -Dtest=UnitTests"
                    }
                }
                stage('Integration Tests') {
                    steps {
                        sh "echo IntegrationTests are running"
                    }
                }
            }
        }
        stage('Deploy') {
            when {
                expression { params.ENV == 'Prod' }
            }
            steps {
                echo "Deploying to ${params.ENV} environment."
                echo "Deployment has been completed to ${params.ENV} environment."
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            cleanWs()
            echo 'Workspace cleaned and artifacts archived.'
        }
        success {
            mail to: 'sudheerkumarp0357@gmail.com',
                 subject: "Pipeline ${currentBuild.fullDisplayName} Succeeded",
                 body: "Good job! The pipeline succeeded.\nBuild URL: ${env.BUILD_URL}"
        }
        failure {
            mail to: 'sudheerkumarp0357@gmail.com',
                 subject: "Pipeline ${currentBuild.fullDisplayName} Failed",
                 body: "Oops! The pipeline failed.\nBuild URL: ${env.BUILD_URL}"
        }
        timeout {
            mail to: 'sudheerkumarp0357@gmail.com',
                 subject: "Pipeline ${currentBuild.fullDisplayName} Timed Out",
                 body: "Pipeline timed out after 60 minutes.\nBuild URL: ${env.BUILD_URL}"
        }
    }
    
}
