# CY Capstone Project By Team 1

## Project 

This project is intended to apply the MITRE ATT&CK framework and tools to map a cyber incident and then use the Caldera tool to complete an emulation plan. Included into team's tasks is the deployment in the Cloud of a mock-up scenario that represents the system compromised in the cyberincident studied. 

As a result of the project, with the Adversary Emulation Plan is needed to elaborate Threat Report of the incident analized (including a STIX2.1 JSON representation). 

You can find the 
1. Presentation docs here: [Docs](https://github.com/capstone-cy-team-1/capstone-cy-team-1.github.io/tree/main/public)
2. Organization housing all the repositories: [Capstone-cy-team-1](https://github.com/capstone-cy-team-1/)
3. Scrum Board here: [Team-1 Scrum Board](https://github.com/orgs/capstone-cy-team-1/projects/1)

## Proposal 

Vulnerability: Supply Chain Attack(s)
Idea: Build an infrastructure for an organization that heavily relies on a JavaScript environment. This environment will be utilized for implementing the Dependency Confusion attack using custom scripts and the Caldera tool.

## Status

![](/public/chart.png)

Sprint-1 [3rd Feb - 15th Feb]:  Completed 

Sprint-2 [13th Feb - 20 March]:  Ongoing 


## Overview

The implementation is divided into two halves.
1. Organization Network + Employee Machine
2. Adversary’s Network

![](/public/file_1.drawio.png)

**Organization Network**

This segment is a virtual network deployed in GCP with three servers (Linux) with NodeJS and NPM installed. The servers, namely the Production, Deployment/Staging, and Repository, are the only components in the organization environment.

Production and Deployment servers will run the vulnerable web application we will be building. The web application will have vulnerabilities 
“None” algorithm attack:  This will allow us to do account takeovers and leak the private package that the admin has stored in their account.
Dependency Confusion: This is another supply chain attack that we will use to allow code injections and execute the malicious payload in the “development servers.”

The production server would be allowed to have DNS lookups, whereas the Development server will have outbound HTTP(s) and DNS lookup capabilities.

As shown in the diagram, we also have an employee machine, which can be outside or inside the organization's network. The idea is to compromise this machine, provided it tries to download the private package.


**Adversary’s Network**

This segment is a virtual network deployed in GCP with two servers (Linux). The servers, namely the Attacker’s DNS and Caldera servers, are the only components in the adversary’s environment.

The attacker’s DNS Server will run a Bind9 service with our custom domain name `0xparthhackerone.me`. This is where the callbacks will be made whenever the victim machine does a DNS lookup on our domain.

Caldera Server will be running on a Linux-based machine which will help us demonstrate the execution of the agent script.

## Organization Network

Machine requirements:

```bash
# 3 Linux Machines
RAM: 4GB or more
Storage: Min
```

The 3 machines require 

```
Node: v18.11.0
npm: v8.19.2
pm2: 5.2.2

# The versions can change depending on the time of implementation
```

## Adversary’s Network

## Attack Mock Run Up Screenshots
- In this scenario, we are forcing victim machine to fetch malicious package hosted by attacker, instead of privately hosted package. In the below image we can see that when machine tries to pull package from NPM public website it gives status code 404 ( Not Found) and 200 (OK) when fetched from internal repository.

![MicrosoftTeams-image (1)](https://user-images.githubusercontent.com/86850255/225090333-164a39e2-faa4-4a91-99a3-f8edc56598be.png)

- Once we identify that package it not hosted on NPM, as an attacker we will try to host malicous package with same name used by victim and publish that on NPM.
![MicrosoftTeams-image (2)](https://user-images.githubusercontent.com/86850255/225091438-af3c8d3f-fcdc-4de2-8957-f15211b64987.png)

![MicrosoftTeams-image](https://user-images.githubusercontent.com/86850255/225091624-1d9a0682-8c8b-4f1e-9578-5fe1ecabe281.png)

- Once we host our malicious package successfully, and when victim would try to fetch the package again, this time system will fetch from NPM public repository which is hosted by attacker.

![MicrosoftTeams-image](https://user-images.githubusercontent.com/86850255/225093110-5d41f2a5-954d-452e-ac23-0df108f3ed0d.png)

- Once victim fetches the attacker malicous package and install it, the Pre-script command in Package.json file would give us a callback on our DNS server and a successfully agent in Caldera.

![MicrosoftTeams-image install](https://user-images.githubusercontent.com/86850255/225094220-643213d7-0e4b-43ac-a04b-f3f7ab9e96d5.png)

![MicrosoftTeams-image (2)](https://user-images.githubusercontent.com/86850255/225094406-514d629b-ed8b-4a56-8ffc-a101c53e4e9f.png)

![MicrosoftTeams-image (4)](https://user-images.githubusercontent.com/86850255/225094442-25beb18b-0aac-4792-8fd1-2cf9856c9152.png)






