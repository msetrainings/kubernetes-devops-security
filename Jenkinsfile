pipeline {
  agent any

  stages {
      stage('Build Artifact') {
            steps {
              sh "mvn clean package -DskipTests=true"
              archive 'target/*.jar' //so that they can be downloaded later
            }
        }
      stage('Unit tests'){
        steps{
          sh "mvn test"
        }
        post{
          always{
            junit 'target/surefire-reports/*.xml'
            jacoco execPattern: 'target/jacoco.exec'
          }
        }
        stage('Docker build and publish'){
          steps{
            withDockerRegistry([credentialsId: "CREDS-DOCKER-HUB", url: ""]) {
            sh 'printenv'
            sh 'docker build -t siddharth67/numeric-app:""$GIT_COMMIT"" .'
            sh 'docker push siddharth67/numeric-app:""$GIT_COMMIT""'
            }
          }
        }
      }   
    }
}
