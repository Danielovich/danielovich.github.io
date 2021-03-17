---
layout: post
title: "Writing a VSTS Task leveraging a dotnet program for Azure DevOps"
categories: Azure
author:
- Daniel
published: true
---

When writing a custom VSTS task I would recommend using the https://github.com/microsoft/azure-pipelines-task-lib.

I will use the PowerShell version of the SDK.

In PowerShell

```powershell
mkdir simpletaskdotnet

cd simpletaskdotnet
```

And then we want to consume the SDK and change the structure of the task folders.

```powershell
Save-Module -Name VstsTaskSdk -Path .\

rename-item -path vststasksdk -newname ps_modules 

cd ps_modules

move-item .\0.11.0\ -destination vststasksdk

```

Now your folder structure should look like this:

```powershell
simpletaskdotnet
    ps_modules
        vststasksdk
```

So far so good.

Now you can go ahead and create a new console program in visual studio, name it what ever you want, and save it to the simpletaskdotnet folder.

In program.cs go ahead and write a program that takes in potential 2 arguments.

```csharp
using System;

namespace SomeProgram
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine(args[0]);
            Console.WriteLine(args[1]);
        }
    }
}
```

Build it 

```cmd
dotnet build -c release
```

When you wish to upload and use you task on Azure DevOps or your own build server you need to supply a task.json file and a manifest file as well.

The format of these files are pretty straight forward. One for our simpletaskdotnet looks like this.

Task.json

```json
{
  "$schema": "https://raw.githubusercontent.com/Microsoft/azure-pipelines-task-lib/master/tasks.schema.json",
  "id": "7a3fbeab-c278-458e-94f8-f8cb044d10e9",
  "name": "simpletaskdotnet",
  "friendlyName": "simpletask dotnet",
  "instanceNameFormat": "simpletask dotnet",
  "description": "simpletask dotnet",
  "helpMarkDown": "",
  "category": "Build",
  "author": "Daniel Frost",
  "version": {
    "Major": 0,
    "Minor": 6,
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
    },
    {
      "name": "directory",
      "type": "string",
      "label": "Azure DevOps predefined variable",
      "defaultValue": "$(System.TeamProject)",
      "required": true,
      "helpMarkDown": "Directory",
      "groupName": "inputGroup"
    }
  ],
  "execution": {
    "Process": {
      "target": "dotnet",
      "argumentFormat": "$(currentDirectory)\\SomeProgram\\SomeProgram\\bin\\Release\\netcoreapp3.1\\SomeProgram.dll /name:\"$(name)\" \"$(directory)\" ",
      "workingDirectory": "$(currentDirectory)"
    }
  }
}
```

Vss-extension.json

```json
{
    "manifestVersion": 1,
    "id": "simpletaskdotnet",
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
                "name": "simpletask dotnet"
            }
        }
    ]
}
```

Now we are actually to create our task on our own instance of Azure DevOps. My instance is called https://dev.azure.com/danielfrostfyi.

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

The code for the task you can find here https://github.com/Danielovich/vsts-task-dotnet