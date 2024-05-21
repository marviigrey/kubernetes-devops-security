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
      stage('junit-test') {
        steps {
          sh 'mvn test-compile org.pitest:pitest-maven:mutationCoverage'
        }
        post{
          always{
            pitmutation mutationStatsFile: '**/target/pit-reports/**/mutations.xml'
          }
        }
      }
      stage('build && SonarQube analysis') {
            steps {
                withSonarQubeEnv('sonar') {
                    // Optionally use a Maven environment you've configured already
                    
                        sh "mvn clean verify sonar:sonar -Dsonar.projectKey=java-app -Dsonar.projectName='java-app'"
                    
                }
            }
        }
      
      }
      
    
  
 }
  
