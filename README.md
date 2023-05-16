https://www.digitalocean.com/community/tutorials/how-to-install-jenkins-on-ubuntu-22-04
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/67cd3d5c-fc4b-45bf-bac7-71fa144a0d9f)
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key |sudo gpg --dearmor -o /usr/share/keyrings/jenkins.gpg
<br/>
<br/>

sudo sh -c 'echo deb [signed-by=/usr/share/keyrings/jenkins.gpg] http://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'

reduce pipeline using caching
https://learn.microsoft.com/en-us/azure/devops/pipelines/release/caching?view=azure-devops
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/13bfd3d5-35b3-4f42-9e62-80933cf2278b)


![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/f90f4f7b-cf10-4a3f-95aa-1f3cb96531b9)
get connection string for devOps pipeline
check userid and password  (mark your password)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/86d402f4-900e-4499-bb08-c791b1f59bf4)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/d43a8f57-add3-4a1d-b433-f3e376b0c0f7)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/304c2494-396e-4d9d-8dbb-a1e3217f20f8)

move connection string in variable
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/bd7c74e2-15b7-49ed-833f-2d5c2c61eebe)

![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/61c85413-28b8-4d92-b991-a782cdbf8426)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/06c7d540-32db-43c9-9d58-ede5df0b83cd)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/99f4eb3e-361e-4b07-b046-a6d19c6e1e39)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/2f7c60ae-9538-417d-bd25-d889f961e9d5)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/8bf73777-4001-4ec5-a476-44ad09ca2d28)
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/c0252aba-0b02-4b03-bcc5-d8cc3778a2bb)
![Uploading image.png…]()

https://www.youtube.com/watch?v=m1By_JO6xTI&list=PLNijTL1qMrSRZKqqvp-rM84TI0-vMT07J&index=2



Predefined Variable:
https://learn.microsoft.com/en-us/azure/devops/pipelines/build/variables?view=azure-devops&tabs=yaml
![image](https://github.com/rajneeshprakashhajela/AzureDevOps-CICD-Pipeline/assets/43515480/7dd407e1-cc6c-4a98-b289-5ecf15aeb48e)

# AzureDevOps-CICD-Pipeline![image](https://user-images.githubusercontent.com/43515480/229700132-257192a0-1212-4543-9dcb-a6a4761cc42b.png)

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
![image](https://user-images.githubusercontent.com/43515480/235286875-4308d6c4-a395-4ad1-87e7-61cc9b964b26.png)
![image](https://user-images.githubusercontent.com/43515480/235286923-abc2844d-58c0-46f0-8d71-2aa283f29cc5.png)

 ![image](https://user-images.githubusercontent.com/43515480/235286949-0d42e54a-f739-4259-946a-dfbd33cdf680.png)
![image](https://user-images.githubusercontent.com/43515480/235286963-8fe3eb32-ed24-4265-8d91-c1ede76c6a61.png)
![image](https://user-images.githubusercontent.com/43515480/235286964-c95a35c6-58d7-4fc9-b9c3-781e39cfcceb.png)
 ![image](https://user-images.githubusercontent.com/43515480/235286991-528026c8-e90a-4ac1-9f43-4baaf2c150d1.png)

![image](https://user-images.githubusercontent.com/43515480/235287553-0ffc4194-01ab-4268-b2a2-783e9e326d0b.png)
![image](https://user-images.githubusercontent.com/43515480/235287597-7018c9f6-b319-420e-a173-463626ccda8a.png)
![image](https://user-images.githubusercontent.com/43515480/235287614-55da7dce-4248-45b9-bf03-c661c2f35767.png)
Lint
 ![image](https://user-images.githubusercontent.com/43515480/235287877-f39cafa3-2cd0-4213-a205-57e4c8c1afdc.png)

 Sonarqube in azure devops Pipeline
 
![image](https://user-images.githubusercontent.com/43515480/235288109-d43ec1ab-de7c-484d-97df-e04e9b8c9ed4.png)
 ![image](https://user-images.githubusercontent.com/43515480/235288137-6038f8c8-7d28-4f14-bef7-305c13de9110.png)
![image](https://user-images.githubusercontent.com/43515480/235288224-f9f68e51-a48d-41c0-adeb-067159559538.png)
![image](https://user-images.githubusercontent.com/43515480/235288243-598b8720-02fc-4b8d-9e15-00809c1a3f62.png)
![image](https://user-images.githubusercontent.com/43515480/235288785-ffe9c6a8-7795-44b3-9aa5-e34627367ddf.png)
