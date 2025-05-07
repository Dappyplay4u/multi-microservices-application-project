Microservice Web Application Project Architecture
Online Shopping Application: This online shopping application was architected and built using cloud-first related principles and methodologies that promotes the adoption of application management strategies such as Microservices. The Online Shopping Application consists of about 11 microservices.

![Project Arch](<[K8S Project] Multi-Service Application Project Arch.png>)

Jenkins Microservices MultiBranch CI/CD Pipeline Automation Arch | One

![CICD ARCH1](<[CI-CD Arch 1] Microservices CI-CD-1.png>)

Jenkins Microservices MultiBranch CI/CD Pipeline Automation Arch | Two

![CICD ARCH2](<[CI-CD Arch 2] Microservices CI-CD-2.png>)

Download the Source Code of each microservice, from their respective branches from this Repository https://github.com/Dappyplay4u/multi-microservices-application-projects.git
And Push the Code based on the Microservice to the specific Branch you Created for that Service.

### Pipeline creation (Make Sure To Make The Following Updates First)
- UPDATE YOUR ``Jenkinsfiles...``

- Update your `Frontend Service` - `OWASP Zap Server IP` and `EKS Worker Node IP` in the `Jenkinsfile` on `Line 100`
  - `NOTE` to update the `Frontend Service`, you must `Switch` to the `Frontend Branch`
- Update the `EKS Worker Node IP` with yours in the `Jenkinsfile` on `Line 100`
<!-- sh 'ssh -o StrictHostKeyChecking=no ubuntu@35.90.100.75 "docker run -t zaproxy/zap-weekly zap-baseline.py -t http://44.244.36.98:30000/" || true' -->

- Update your `Slack Channel Name` in the `Jenkinsfiles...` - `All Microservices` on `Line 126`
<!-- slackSend channel: '#all-minecraftapp', -->


- Update `SonarQube projectName` of your Microservices in the `Jenkinsfiles...` - `All Microservices`
- Update the `SonarQube projectKey` of your Microservices in the `Jenkinsfiles...` - `All Microservices`
- Update the `DockerHub username` of your Microservices in the `Jenkinsfiles...` - `All Microservices`, provide Yours
- Update the `DockerHub username/Image name` in all the `deployment.yaml` files for the different environments `test-env` and `prod-env` folders across `Every Single Microservice Branch`