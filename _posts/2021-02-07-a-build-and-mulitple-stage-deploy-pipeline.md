---
layout: post
title: "Build pipeline and using a multistage piple for deployment on Azure DevOps"
categories: azure-devops
author:
- Daniel
published: true
---

Build pipeline yaml.

Be aware that there are no tests being run nor a restore. So it's really basic. But you get the gist of it.

```yaml
name: $(majorMinorVersion).$(semanticVersion)

pool:
  vmImage: 'windows-latest'

variables:
  majorMinorVersion: '1.0'
  semanticVersion: $[counter(variables['majorMinorVersion'], 0)]
  buildConfiguration: 'Release'

trigger:
- master

steps:
- task: DotNetCoreCLI@2
  inputs:
    versioningScheme: byBuildNumber
    command: build
    projects: '**/*.sln'
    arguments: '--configuration $(BuildConfiguration)'

- task: DotNetCoreCLI@2
  inputs:
    command: publish
    publichWebProjects: true
    projects: '**/*.csproj'
    arguments: '--output $(Build.ArtifactStagingDirectory)'
    zipAfterPublish: true
    modifyOutputPath: true

- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: $(Build.ArtifactStagingDirectory)
    ArtifactName: bookerArtifact
```

When you want to do multistage deployments in Azure DevOps you simply do a yaml file with more stages
in it.

You need to manually, by GUI in Azure Devops, set the Approvals for who can promote the deployments in 
the stages you wish to have.

```yaml
trigger:
  - master

# pool used in pipeline
pool:
  vmImage: 'windows-latest'

stages:
- stage: 'test'
  displayName: 'deploy to test'
  jobs:
  - deployment: TestDeploy
    environment: 'bookerapp-test'
    strategy:
      runOnce:
        deploy:
          steps:

          - task: DownloadPipelineArtifact@2
            displayName: 'Downloading artifact'
            inputs: 
              source: 'specific'
              project: 'booker'
              pipeline: 1 #ID of pipeline
              artifactName: bookerArtifact
              patterns: '**/*.zip'
              path: $(System.ArtifactsDirectory) 

          - task: PowerShell@2
            displayName: 'Debug parameters'
            inputs:
              targetType: Inline
              script: |
                tree /f $(System.ArtifactsDirectory)

          - task: AzureWebApp@1
            displayName: 'Azure Web App Deploy'
            inputs:
              azureSubscription: 'azuresub'
              appName: 'bookerapp'
              slotName: 'test'
              package: $(System.ArtifactsDirectory)/**/*.zip

- stage: 'production'
  displayName: 'deploy to production'
  jobs:
  - deployment: ProductionDeploy
    environment: 'bookerapp-production'
    strategy:
      runOnce:
        deploy:
          steps:

          - task: DownloadPipelineArtifact@2
            displayName: 'Downloading artifact'
            inputs: 
              source: 'specific'
              project: 'booker'
              pipeline: 1
              artifactName: bookerArtifact
              patterns: '**/*.zip'
              path: $(System.ArtifactsDirectory) 

          - task: PowerShell@2
            displayName: 'Debug parameters'
            inputs:
              targetType: Inline
              script: |
                tree /f $(System.ArtifactsDirectory)

          - task: AzureWebApp@1
            displayName: 'Azure Web App Deploy'
            inputs:
              azureSubscription: 'azuresub'
              appName: 'bookerapp'
              package: $(System.ArtifactsDirectory)/**/*.zip
```

