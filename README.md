This is a demo of CI/CD pipeline using jenkins which build, test and deploy a dotnet webApp.
Developer push the code to the SCM (github) which contains the jenkinsfile which first, pulls the code from git repo & then follow the pipeline script sequentially.
git repo : https://github.com/vinay301/jenkins-demo.git

This pipeline includes some DevSecOps tools like OWASP Dependency Check, Sonarqube, Trivy Scan.

To run this pipeline
1. Must have jenkins, sonarqube, trivy over linux
2. Configure sonarqube, OWASP docker tools on jenkins, as this repo is public, it doesn't require github credentials
3. To check Automated Build, we need to configure NGROK on our linux and also provide the generated domain url on github webhooks

Steps to setup NGROK : 
1. Download NGROK on linux
2. Provide your unique AuthToken
3. Then provide your domain
4. Make sure that your domain's tunnel is running
5. My pipeline url : `https://cheaply-promoted-orca.ngrok-free.app` you can access my jenkins pipeline from this url but it requires my credentials
