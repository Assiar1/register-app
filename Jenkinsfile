pipeline{
  agent{ label 'Jenkins-Agent' }
  tools{
    jdk 'java17'
    maven 'Maven3'
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
                sh "mvn sonar:soanr"
            }
        }
      }
    }
    
  
  }
}
