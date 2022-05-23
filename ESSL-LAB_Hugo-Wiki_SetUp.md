# Instructions
## Preparations
### Create GitHub Account
* create GitHub account with name and password
* follow the instructions available on the Internet for creating a *secret*
* save guard the secret for use later in the installation

### Create DockerHub Account
* create DokcertHub account with name and password
* follow the instructions available on the Internet for creating a *secret*
* save guard the secret for use later in the installation

### Install Flux
* follow instructions on [Get Started with Flux](https://fluxcd.io/docs/get-started/)
* if "`flux check --pre`" command fails, make sure '*~/.kube/config*' is available
* include [Clone the git repositoy](https://fluxcd.io/docs/get-started/#clone-the-git-repository) step and then stop following the '*get started*' instructions

## Set-Up ESSL-Lab Wiki-System
### Create Project on GitHub
* create the project on GitHub
* connect from VSC to GitHub project
  
### Import ESSL-Lab Wiki-Page Data
* copy files into project root director on local machine
* unzip and copy '*CICD.yml*' file to '*/path/to/yaml/file*'
* modify '*CICD.yml*' file to apply values specific to ESSL-Lab
* make sure VSC work-space is connected to GitHub repo
* commit and push files to GitHub repo

### Monitor GitHub CI/CD Pipeline
* watch the execution of the CI/CD pipeline
* go to DockerHub and verify that the Hugo image has been pushed

### 'Manually' Deploy ESSL-Wiki to ESSL-Cluster
* unzip and store 'ESSL-Wiki' K8s manifests in appropriate directory
* modify K8s manifest files to apply values specific to ESSL-Lab
* run 'kubectl apply' command to evaluate K8s manifest files
* verify the '*Hugo-Wiki*' K8s pods running
* run '*kubectl port-forward*' command
* access '*Hugo-Wiki*' landing page

## Automate Image Updates to GIT
* follow instructions on [Automate image updates to Git](https://fluxcd.io/docs/guides/image-update/)
* re-run the "`flux bootstrap github`" command with the the extra components
* subsequently follow the '*Deploy a demo app*' instructions, but apply modifications required for *ESSL-Wiki*
* note, there will be steps that need to be skipped!!!
* use the recommended steps to verify FLUX updating the *ESSL-Wiki* image
* try to access the *ESSL-Wiki* landing page