---
layout: post
title: "Writing a VSTS Task for Azure DevOps"
categories: Azure
author:
- Daniel
published: true
---

When writing a custom VSTS task I would recommend using the https://github.com/microsoft/azure-pipelines-task-lib.

I will use the PowerShell version of the SDK.

In PowerShell

```powershell
mkdir simpletask

cd simpletask
```

And then we want to use the SDK and rename it sp the structure matches

```powershell
Save-Module -Name VstsTaskSdk -Path .\

rename-item -path vststasksdk -newname ps_modules 

cd ps_modules

move-item .\0.11.0\ -destination vststasksdk

```

Now your folder structure should look like this:

```powershell
simpletask
    ps_modules
        vststasksdk
```

Open VS Code and open the folder simpletask and then create a new file called task.ps1.

```powershell
[CmdletBinding()]
param()

$name = Get-VstsInput -Name 'name'

Write-Host "Hello " $name
``` 

So far so good.

Now we can try to execute the task. In PowerShell navigate to your simpletask directory.

And execute

```powershell
Import-Module .\ps_modules\VstsTaskSdk
Invoke-VstsTaskScript -ScriptBlock { .\task.ps1 }
```

Next time you run this command PowerShell will have cached the module and also the parameter "name" that you supplied. You might have had to also supply a culture setting, which I have no idea why you get prompted for.

Anyways, this becomes tedious to debug like this, so let's make a debug file instead.

Create a file called debug.ps1. And that file you can enter

```powershell
Import-Module .\ps_modules\VstsTaskSdk

$env:INPUT_name = "Daniel"

Invoke-VstsTaskScript -ScriptBlock { .\task.ps1 }
```

The obvious here is that you can have parameters which for inputs (task.json) you would like to provide.

```powershell
$name = Get-VstsInput -Name 'name'
```

This is a great convention for when you wish to debug these scripts locally and without a build or release agent present.

You can set different types of variables by using the $env: varibale type. Look for more here https://github.com/microsoft/azure-pipelines-task-lib/blob/master/powershell/Docs/TestingAndDebugging.md 

When you wish to upload and use you task on Azure DevOps or your own build server you need to supply a task.json file and a manifest file as well.

The format of these files are pretty straight forward. One for our simpletask looks like this.

Task.json

```json
{
    "$schema": "https://raw.githubusercontent.com/Microsoft/azure-pipelines-task-lib/master/tasks.schema.json",
    "id": "7a3fbeab-c278-458e-94f8-f8cb044d10d9",
    "name": "simpletask",
    "friendlyName": "simpletask",
    "instanceNameFormat": "simpletask",
    "description": "simpletask",
    "helpMarkDown": "",
    "category": "Build",
    "author": "Daniel Frost",
    "version": {
        "Major": 0,
        "Minor": 2,
        "Patch": 0
    },
    "groups": [
        {
            "name": "inputGroup",
            "displayName": "Source",
            "isExpanded": true
        }
    ],
    "inputs": [
        {
            "name": "name",
            "type": "string",
            "label": "Your name",
            "defaultValue": "",
            "required": true,
            "helpMarkDown": "Your name",
            "groupName": "inputGroup"
        }
    ],
    "execution": {
        "PowerShell3": {
            "target": "task.ps1",
            "platforms": [
                "windows"
            ],
            "workingDirectory": "$(currentDirectory)"
        }
    }
}
```

The extension manifest file is for the vsix package we will create in a few moments.

Call the extension file vss-extension.json

```powershell
{
    "manifestVersion": 1,
    "id": "simpletask",
    "name": "Daniels Build and Release Tools",
    "version": "0.0.1",
    "publisher": "Daniel Frost",
    "targets": [
        {
            "id": "Microsoft.VisualStudio.Services"
        }
    ],    
    "description": "Tools for building/releasing with Fabrikam. Includes one build/release task.",
    "categories": [
        "Azure Pipelines"
    ],
    "icons": {
        "default": "images/extension-icon.png"        
    },
    "files": [
        {
            "path": "."
        }
    ],
    "contributions": [
        {
            "id": "custom-build-release-task",
            "type": "ms.vss-distributed-task.task",
            "targets": [
                "ms.vss-distributed-task.tasks"
            ],
            "properties": {
                "name": "simpletask"
            }
        }
    ]
}
```

Now we are actually to creat our task on our own instance of Azure DevOps. My instance is called https://dev.azure.com/danielfrostfyi.

You need to create a personal access token for your instance of azure devops. 

Before you can create and upload the task we must first create the vsix package.

From within PowerShell run these commands, one at a time.

```powershell
npm i -g tfx-cli

. $PROFILE

tfx extension create --manifest-globs vss-extension.json

tfx build tasks create 

#fill out the info

tfx build tasks upload --task-path .

#you might have to login to your instance of Azure DevOps 
```

Now you can login to Azure devops in a browser and create a pipeline, and from that pipeline you can start using you custom task.

The code for the task you can find here https://github.com/Danielovich/custom-vsts-task 