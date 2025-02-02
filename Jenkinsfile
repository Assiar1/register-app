pipeline{
  agent{ label 'Jenkins-Agent' }
  tools{
    jdk 'java17'
    maven 'Maven3'
  }

  environment {
    APP_NAME = "register-app-pipeline"
    RELEASE = "1.0.0"
    DOCKER_USER = "assiar1"
    DOCKER_PASS = 'dockerhub'
    IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
    IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
}


  
  stages{
    stage("Checkout from SCM"){
      steps{
        git branch: 'main', credentialsId: 'Github', url: 'https://github.com/Assiar1/register-app'
      }
    }
    
    stage("Build Application"){
        steps{
            sh "mvn clean package"
        }
    }
    
    stage("Test Application"){
        steps{
            sh "mvn test"
        }
    }

    stage("SonarQube Analysis"){
        steps{
            script{
              withSonarQubeEnv(credentialsId: 'jenkins-sonarqube-token'){
                sh "mvn sonar:sonar"
            }
        }
      }
    }

     stage("Quality Gate"){
        steps{
            script{
              waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonarqube-token'
            }
        }
      }

    stage("Build & Push Docker Image") {
    steps {
        script {
            sh 'docker ps'
        }
    }
}

                                          
    
  
  }
}
