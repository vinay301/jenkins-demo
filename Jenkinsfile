pipeline {
  agent any
  tools {
    maven 'maven'
    jdk 'JDK17'
  }
  environment {
   
    sonarScanner = tool 'sonar-scanner' 
    APP_NAME = "jenkins-demo"
    RELEASE = "1.0.0"
    DOCKER_USER = "vinaysharmadev"
    DOCKER_PASS = "docker-cred"
    IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}" 
    IMAGE_TAG = "${RELEASE}" + "-" + "${BUILD_NUMBER}"
  }
  
  triggers {
    // Trigger the pipeline on every commit to the master branch
    githubPush()
  }
  
  stages {
    stage('Git Checkout') {
      steps {
        git 'https://github.com/vinay301/jenkins-demo.git'  
      }
    }

    stage('Grant Docker Permissions') {
      steps {
        sh 'sudo chmod 666 /var/run/docker.sock'
        }
    }

    stage('Dotnet Build') {
            agent {
                docker {
                     image 'mcr.microsoft.com/dotnet/sdk:7.0'
                     args '--user root' 
                     reuseNode true
                }
            }
            steps {
                sh 'dotnet build "$WORKSPACE/JenkinsDemo.sln" --configuration Release'
                sh 'echo "Into the Build Stage"'
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
    
    stage('Build & Push Docker Image') {
      steps {
        script{
            withDockerRegistry(credentialsId: 'docker-cred') {
                docker_image = docker.build "${IMAGE_NAME}"
            }
             withDockerRegistry(credentialsId: 'docker-cred') {
                docker_image.push("${IMAGE_TAG}")
                docker_image.push('latest')
            }
        }
      }
    }
    
    stage('Trivy Scan'){
        steps{
            sh "trivy image ${IMAGE_NAME}"
        }
    }
    
    stage('Run Docker Container'){
        steps{
            script{
                
                  // Stop and remove the existing container if it exists
                  sh 'docker rm -f jenkinsPipeLineDemo || true'
                  docker.image("${IMAGE_NAME}:${IMAGE_TAG}").pull()
                  //Got the error here - Administrators can decide whether to approve or reject this signature.
                  // def myContainer = docker.container('jenkinsPipeLineDemo').withRun('-p 1000:80') 
                   
            }
        }
        post {
            always {
              // Run the Docker container with port mapping
              sh 'docker run -d -p 1000:80 --name jenkinsPipeLineDemo ${IMAGE_NAME}:${IMAGE_TAG}'
            }
        }
    }
    
  }
}

