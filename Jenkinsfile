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
            withDockerRegistry([credentialsId:'docker-login', url: '']) {
            sh 'printenv'
            sh 'docker build -t marviigrey/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push marviigrey/numeric-app:""$GIT_COMMIT""'

              }
          
          }
    }
    stage('deploy to kubernetes') {
      steps {
        withKubeConfig([credentialsId: 'kubeconfig']) {
            sh "sed -i 's#replace#marviigrey/numeric-app:${GIT_COMMIT}#g' k8s_deployment_service.yaml"
            sh "kubectl create -f k8s_deployment_service.yaml"
        }
      }
    }
       
        }
  }
