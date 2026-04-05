---
title:  "How I did it: Home Hosting with Komodo"
date:   2026-04-03 10:50:00
categories: [home-hosting]
tags: [dietpi, linux, komodo, how-i-did-it]
---

I was a user of [Portainer](https://www.portainer.io/) for a long long time, but it always dissapointed me with payable futures or even with some open security issues by default (maybe I'm wrong with the latest, will try to explain it later). So, I was looking for somethin new, something maybe better or even easier to use. I'm not sure, if easier to use would be even possible as Portainer was really simple to install and easy to use, but still, I was not fully happy with it. 

## 1. The story
I came over the similar product as Portainer called [Komodo](https://komo.do/) and at the first glance I was not convinced, at least not from their home page. Something else were their [Github page](https://github.com/moghtech/komodo) that give me little bit more of the thinking and I gave in and did a try-run for it, next to the already existing Portainer. 

## 2. Install
The installation of it, is little bit more advanced as compared to the Portainer, but still pretty simple and really no need to be an advanced user for it. I followed the instructions on this [link](https://komo.do/docs/setup/mongo), changed some settings and had it up and running in <5 minutes. 

## 3. Sync
Till recently I used Komodo in the same way as Portainer. I used or UI to define the containers or docker-compose on the UI to configure everything and run it. It was quite a work to configure all the containers and all that I use on different locations and different machines, so I was wondering what would be the option that would help me here. First I tried to copy-paste the docker-compose files, that worked out like a charm, but who like to copy-paste nowadays? No one, right? So the last option that I explored under Komodo was the sync, it's available under the "Syncs" menu on the UI! And how it works? It works like a charm. You define the requirements in the [toml file](https://toml.io/en/), push the file on Git, sync it and you have something like IaC, and it works. 

### 3.1 Example
Let's play around with an example and define simple container with toml, push it to the Github and check out if it will pop-up in our Komodo. 

#### 3.1.1 Github provider
To make all the things work together, we need to define first the Github provider under the settings. 
Open `Settings` and select `Providers` in the menu. Click on the `New Account` and fill it in with data. For Github use domain `github.com`, username is your Github username and Token is PAT that you will need to create. I reccoment here, to create dedicated PAT for Komodo only! Ok, this was the first step, we will verify if everything works in the one of the next steps. 
#### 3.1.2 Define toml 
Let's define the simple deployment in the toml file, for this example we will use `nginx`.
```toml
[[deployment]]
name = "nginx"
[deployment.config]
server = "<name_of_your_servcer>"
file_contents = """
## Add your compose file here
services:
  nginx:
    container_name: nginx
    image: nginx:stable-alpine3.23-perl
    ports:
      - "8080:80"
    restart: unless-stopped
"""
```
Save the content to the `nginx.toml` file, commit it and push it to the Github!
#### 3.1.3 Configure syncs
Now, we are ready to configure the `syncs` in our Komodo. Open the `Syncs` in the menu and create a new Sync, give it a name in the fist step and configuration will be promped later. When the first step is done, you should get an option to choose the mode, for our case let's use the `Git repo`. Select the git provider, in our case github, selec the account and fill in the `Repo` (this is the repository to which you pushed the `nginx.toml` file in previous step). If you will have multiple configus for different servers in the same repo, I reccomentd also to set the `Resource Paths`. The rest I left as on default and save the settings. Hit `Refresh` button on the top and under the `Info` tab you should be able to see the files from the Github. The top bar will change the color to orange and be marked as `pending` in case there are some sync differences between your comodo configuration and the github. Switch to the `Execute` and you could see the diff in between, to apply the config just press the `Execute Change` button and the `nginx` should be available under the deployments. Just need to pull the image and deploy it. 

## 4. Conclusion
Dealing with the backups and deplying same apps over multiple servers become quite simple with the `Syncs` option.
