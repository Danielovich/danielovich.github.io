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
# Add a build possibility. You might want to skip this part if you 
# are e.g. doing CI with an external build engine such as Jenkins or Azure DevOps

FROM mcr.microsoft.com/dotnet/sdk:3.1 AS build-env

#Set the work dir in the Container!

WORKDIR /src

# Copy the csproj files to "." which will point to the WORKDIR (/src)
# Run restore
COPY *.csproj .
RUN dotnet restore Frost.RssReader.Web.csproj

# Copy the remaining files from the root of our project space "." 
# into "." (the WORKDIR in the container "/src")
# run publish 
COPY . .
RUN dotnet publish -c Release -o out

# FROM another layer to work with
FROM mcr.microsoft.com/dotnet/core/aspnet:3.1

# COPY from the buid step, aliased image layer, with 
# "/src/out" to destination in container, on "."
COPY --from=build-env /src/out .

# Tell the image container to start
ENTRYPOINT ["dotnet", "Frost.RssReader.Web.dll"]
```

I kick in the docker build with a tag and use the latest Git commit id as a tag

```docker
docker build . -t frostrss:5982ff5db7aa600a9535e757fc5bcf02fb09f9ca
```

And then I can do docker run with the following args (27beaac719ab is the image ID)

```docker
docker run -p 5000:80 --name frostrsscontainer 27beaac719ab
```