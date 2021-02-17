---
layout: post
title: "docker push authentication required"
categories: docker
author:
- Daniel
published: true
---
This is happening on a Windows 10 box that has been sleeping for some time (hours). With Docker Desktop. Docker version 20.10.2.

I started to see "authentication required" whenever I had done a "docker push" command. The command would run for some time and it would appear that 
some layers were being pushed, but suddenly the command would crash into a state of "authentication required".

```docker
16-Feb-2021 01:35:17 | 340dd2d83760: Waiting
16-Feb-2021 01:35:17 | d4b742028f8f: Layer already exists
16-Feb-2021 01:35:17 | 9f8ebdc8fafb: Pushing [===================================> ] 128.8 MB/179.1 MB
16-Feb-2021 01:35:17 | b92dd801d461: Waiting
16-Feb-2021 01:35:17 | 18f9b4e2e1bc: Waiting
16-Feb-2021 01:35:19 | unauthorized: authentication required
```

Okay I thought. What's up docker, wanna play hard ?

First things first I guess. First I tried to make sure my tag was correct, which is a thing when you work with docker hub at least.

You must tag the image like so, if you want to push it to your repo on docker hub, that is.

```console
docker build -t danielfrostdk/frostrss:latest .
```

So tried that with no luck. Specifically I did

```console
docker build -t danielfrostdk/frostrss:latest .
docker push danielfrostdk/frostrss:latest
```

Same result - "authentication required".

Let's logout of docker and login with specifics, and not some potentially cached credentials on the machine.

```console
docker logout
docker login -u danielfrostdk
Password ...
Login succeeded

docker push danielfrostdk/frostrss:latest
...
...
...
unauthorized: authentication
```

Fuck me I thought, it's not giving me anything! So let's try to restart docker. Right click on the whale tray icon and hit Restart.

Same result. Doubble check for typo's. Same result. 

Then I remembered that I had previously (weeks ago) experienced some strange behaviour when starting docker containers; "docker run ..."

It had started up good enough but when I started to interact with the actual application on the exposed endpoint e.g: [POST]https://localhost:5000/reader it began
telling me "Request date header too old". Get the fuck outte here...too old ? I just send off the request, no way it's too old.

Then I started to think about in which context I was executing this container in, and being executed based on the WSL2 subsystem. What if the subsystem had halted somehow ?
What if the subsystem's clock was fucked and more importantly, why would it be ?

Back to my docker push issue. I opened a admin cmd and types in:

```console
wsl date

Mon Feb 15 09:49:23 UTC 2021
```

Ok, so the date is way off.

Might that be an issue ? I then shutdown wsl in an admin cmd, by:

```console
wsl --shutdown
```

Then my docker desktop told me thatthe WSL2 backend had been stopped unexpectedly. I didn't touch it.

Then I simply tried docker push again, and then it ran right through.

```console
docker push danielfrostdk/frostrss:latest

The push refers to repository [docker.io/danielfrostdk/frostrss]
b619993080e1: Pushed                                                                                                 
464872ddfb9f: Pushed 
507c77ea770e: Pushed                                                                                                 
5a2f05c04b6d: Layer already exists
bb50d1ecf116: Pushed                                                                                                 
9eb82f04c782: Pushed                                                                                                 
latest: digest: sha256:XXXXXXXXXXXXXXX size: 1586
```

This is obviously an issue.  Since I do not use Hyper-V with docker desktop I cannot turn on time syncronization from there, so I guess I will 
have to shutdown wsl whenever I see this issue ?