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
    stage('docker build image') {
          steps {
            withDockerRegistry([credentialsId:'docker-login']) {
            sh 'printenv'
            sh 'docker build -t marviigrey/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push marviigrey/numeric-app:""$GIT_COMMIT""'

              }
          
          }
    }
       
        }
  }
