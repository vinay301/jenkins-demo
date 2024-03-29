pipeline {
    agent any
    tools {
      
       jdk 'JDK17'
    }
     environment {
      SCANNER_HOME = tool 'sonar-scanner'
      scannerHome = tool 'SonarScanner for MSBuild'
    }
    stages {
        stage('Git Checkout') {
            steps {
                git 'https://github.com/vinay301/jenkins-demo.git' 
            }
        }
        stage('OWASP Dependency Check') {
            steps {
                dependencyCheck additionalArguments: ' --scan ./ ', odcInstallation: 'Dep-Check'
                dependencyCheckPublisher pattern: 'dependency-check-report.xml'
            }
        }
       
        
        
        stage('SonarQube Analysis') {
            steps{    
                withSonarQubeEnv(installationName: 'sonar') {
                 bat '''$SCANNER_HOME/bin/sonar-scanner \
                     -Dsonar.projectKey=DotnetCoreAutomatedPipeline \
                     -Dsonar.projectName=DotnetCoreAutomatedPipeline '''
                }
            }
           
      }
        
  }
       
}


