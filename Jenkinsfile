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
        stage('Trivy FS Scan') {
            steps {
              sh "trivy fs ."
            }
        }
        
        
        
        stage('SonarQube Analysis') {
             def scannerHome = tool 'SonarScanner for MSBuild'
            steps{
                
                withSonarQubeEnv(installationName: 'sonar') {
                  bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll begin /k:\"DotnetCoreAutomatedPipeline\""
                  bat "dotnet build"
                  bat "dotnet ${scannerHome}\\SonarScanner.MSBuild.dll end"
                }
            }
           
      }
        
  }
       
}


