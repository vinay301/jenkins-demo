pipeline {
  agent any
  tools {
    maven 'maven'
    jdk 'JDK17'
  }
  environment {
   
    sonarScanner = tool 'sonar-scanner' 
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
      steps {
        withSonarQubeEnv(installationName: 'sonar') {
          sh """${sonarScanner}/bin/sonar-scanner \
            -Dsonar.projectKey=DotnetCoreAutomatedPipeline \
            -Dsonar.projectName=DotnetCoreAutomatedPipeline"""
        }
      }
    }
  }
}

