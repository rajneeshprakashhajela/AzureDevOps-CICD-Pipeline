# AzureDevOps-CICD-Pipeline
![image](https://user-images.githubusercontent.com/43515480/229700132-257192a0-1212-4543-9dcb-a6a4761cc42b.png)

<img width="295" alt="image" src="https://user-images.githubusercontent.com/43515480/229712011-17903b23-b920-4cdc-9246-64f31e351b37.png">


 Stages <br1>
 Stage-1:<br1>
   Task-1: Build Docker Image and push to Azure Container Registry ACR<br1>
   Task-2: Copy kube-manifest files to Build Artifact Directory<br1>
   Task-3: Publish build articats to Azure Pipelines<br1>
   Pipeline Hierarchial Flow: Stages -> Stage -> Jobs -> Job -> Steps -> Task1, Task2, Task3  <br1>

trigger:<br1>
- master<br1>

Variables<br1>
variables:<br1>
  tag: '$(Build.BuildId)'<br1>

stages:<br1>
 Build Stage <br1>
- stage: Build<br1>
  displayName: Build Stage<br1>
  jobs:<br1>
  - job: Build<br1>
    displayName: Build Job<br1>
    pool:<br1>
      vmImage: 'ubuntu-latest'<br1>
    steps: <br1>

    Task-1: Build Docker Image and push to Azure Container Registry ACR<br1>
    - task: Docker@2<br1>
      inputs:<br1>
        containerRegistry: 'manual-aksdevopsacr-svc'<br1>
        repository: 'custom2aksnginxapp1'<br1>
        command: 'buildAndPush'<br1>
        Dockerfile: '**/Dockerfile'<br1>
        tags: |<br1>
          $(tag)<br1>
          $(Build.SourceVersion)<br1>
Publish Artifacts pipeline code in addition to Build and Push          <br1>
    - bash: echo Contents in System Default Working Directory; ls -R $(System.DefaultWorkingDirectory)        <br1>
    - bash: echo Before copying Contents in Build Artifact Directory; ls -R $(Build.ArtifactStagingDirectory)      <br1>  
    # Task-2: Copy files (Copy files from a source folder to target folder)<br1>
    # Source Directory: $(System.DefaultWorkingDirectory)/kube-manifests<br1>
    # Target Directory: $(Build.ArtifactStagingDirectory)<br1>
    - task: CopyFiles@2<br1>
      inputs:<br1>
        SourceFolder: '$(System.DefaultWorkingDirectory)/kube-manifests'<br1>
        Contents: '**'<br1>
        TargetFolder: '$(Build.ArtifactStagingDirectory)'<br1>
        OverWrite: true<br1>
    # List files from Build Artifact Staging Directory - After Copy<br1>
    - bash: echo After copying to Build Artifact Directory; ls -R $(Build.ArtifactStagingDirectory)  <br1>
    # Task-3: Publish build artifacts (Publish build to Azure Pipelines)           <br1>
    - task: PublishBuildArtifacts@1<br1>
      inputs:<br1>
        PathtoPublish: '$(Build.ArtifactStagingDirectory)'<br1>
        ArtifactName: 'kube-manifests'<br1>
        publishLocation: 'Container'<br1>
    

![image](https://user-images.githubusercontent.com/43515480/229738853-7eb87860-f7c3-4eb5-9124-2576cd9e8936.png)
===========

<h1>OWASP </h1>
https://medium.com/adessoturkey/owasp-zap-security-tests-in-azure-devops-fe891f5402a4

<img width="938" alt="image" src="https://user-images.githubusercontent.com/43515480/230543150-29daecdb-c2f2-410f-9fed-4fb29284d19e.png">


![image](https://user-images.githubusercontent.com/43515480/235285410-142d2538-faed-48da-86de-cd74879ccb67.png)
![image](https://user-images.githubusercontent.com/43515480/235286461-daa015af-13b1-451e-993a-d1e21babad0d.png)
![image](https://user-images.githubusercontent.com/43515480/235286550-00b2f0d3-d735-4eeb-aa30-05f047f15414.png)
![image](https://user-images.githubusercontent.com/43515480/235286555-5d8b1efc-4747-40f2-898b-ba35b745f65a.png)
