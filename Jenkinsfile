


pipeline {
    agent any
    /*
    testing
    tools {
        docker "dockerr"
    }
    */

    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('sonar-cred')
        SONAR_ORGANIZATION = 'ayan'
        SONAR_PROJECT_KEY = 'ayan_new'
    }

    stages {
        
        stage('Code-Analysis') {
            steps {
                withSonarQubeEnv('SonarCloud') {
                    sh '''$SCANNER_HOME/bin/sonar-scanner \
  -Dsonar.organization=ayan \
  -Dsonar.projectKey=ayan_new \
  -Dsonar.sources=. \
  -Dsonar.host.url=https://sonarcloud.io '''
                }
            }
        }
       
        
      
       stage('Docker Build And Push') {
            steps {
                script {
                    docker.withRegistry('', 'pekker123') {
                        def buildNumber = env.BUILD_NUMBER ?: '1'
                        def image = docker.build("pekker123/crud:latest")
                        image.push()
                    }
                }
            }
        }
    
       
        stage('Deploy To EC2') {
            steps {
                script {
                     
                        sh "docker run -d -p 3000:3000 pekker123/crud:latest"
                        
                    
                }
            }
        }
        
}
}
