# AzureDevOps-CICD-Pipeline
![image](https://user-images.githubusercontent.com/43515480/229700132-257192a0-1212-4543-9dcb-a6a4761cc42b.png)

trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

variables:
  acrConnection: 'MyAcrServiceConnection'
  imageName: 'sample-image'

steps:
- task: Docker@2
  displayName: BuildAndPushImageToACR
  inputs:
    repository: $(imageName)
    command: 'buildAndPush'
    containerRegistry: $(acrConnection)
    Dockerfile: '**/Dockerfile'
    tags: $(Build.BuildId)
