---
layout: post
title: "A docker file and commands to go with it"
categories: docker
author:
- Daniel
published: true
---

I usually have a single project file which I am about to dockerize and enable for pushing out to whatever container reg' I use.

```docker
FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build-env

WORKDIR /src

COPY *.csproj .

RUN dotnet restore Frost.RssReader.Web.csproj

COPY . .

RUN dotnet publish -c Release -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

COPY --from=build-env /src/out .

ENTRYPOINT ["dotnet", "Frost.RssReader.Web.dll"]
```