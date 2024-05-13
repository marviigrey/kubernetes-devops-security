pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' 
            }
        }
      stage('unit testing') {
            steps {
              sh "mvn test"
            }
            post {
              always {
                junit 'target/surefire-reports/*.xml'
                jacoco execPattern: 'target/jacoco.exec'
              }
            }
            
      }
      stage('SonarQube Analysis') {
    def mvn = tool 'Default Maven';
    withSonarQubeEnv('sonar') {
               sh "${mvn}/bin/mvn clean verify sonar:sonar -Dsonar.projectKey=java-app -Dsonar.projectName='java-app'"
          }
        }
      }
      
    
  
    
    
       
        }
  
