---
layout: post
title:  "Porto Tech Center devops manifest"
author: "Rui Duarte"
date:   2016-03-13 
thumbnail: /img/posts/devops_icon.png
categories: [engineering-quality]
tags: [devops]
permalink: /porto-tech-center-devops-manifest/
---

# What is the DevOps culture

## DevOps is not…

- A certification
- A role
- A set of tools
- A prescriptive process

## DevOps is…

* Having customer satisfaction as the highest priority, through early and continuous delivery of valuable functionality;
* Ensuring system reliability and performance;
* Being preventive and building fast feedback loops to maximize client satisfaction;
* Breaking silos and building bridges between systems, software and project administration:
  * Learn to trust;
  * Understand motivations;
  * Eliminate blame;
  * Keeping focus on the common goal;
* Promote automation throw the perspective of infrastructure as code;
* Embrace smart failure;
* Focus on bottlenecks and flow;
* Reduce (if possible, eliminate) unplanned work;
* Be continuous;
* Form dedicated, cross-functional teams;
* Love transparency;
* Build autonomy, mastery and purpose.

----------------

------------- | -------------
Venn diagram showing DevOps as the intersection of development (software engineering), technology operations and quality assurance (QA) In traditional, functionally separated organizations there is rarely cross-departmental integration of these functions with IT operations. DevOps promotes a set of processes and methods for thinking about communication and collaboration between development, QA, and IT operations. | ![alt text](/img/posts/diagram-01.png "DevOps as the intersection of development")

![alt text](/img/posts/devops_icon.png "DevOps Icon") | 


## DevOps Responsibility

* Automate the server provisioning process to reduce all interactions to the minimum set as possible, once we plug a new server in, it walks itself through all aspects of provisioning to join the fleet without any human involvement.
* Help in every possible way in building scalable infrastructure and applications, that if necessary would grow horizontally and still be able to manage hundreds of billions of request, hundreds of petabytes of user data, and millions of concurrent connections
* Working on improving current application, services and infrastruct performance in order to deliver better end user experience and minimize resources.
* Drive the company through “Disaster Recovery Tests”, were manually or even automatically, it would be possible to turn down pieces of infrastructure and software to test overall resiliency to failures and ultimately also have a full “Disaster Recovery Plan” in case of major failure.
* Design the system and processes that engineers use to deploy their software into production
* Build notification and auto-remediation system to automatically notify and if possible resolve production incidents before escalating them to on-call engineers.

## What we want to achieve

* Non disruptive daily development and deployment cycle with minimum downtime, that that continuously deliver valuable functionality to the end user.
* Operations and development are skills, not roles, should be a culture and should extend to every role inside the company. Delivery teams are composed of people with all the necessary skills.
* Delivery teams run software products – not projects – that run from inception to retirement


> “You Build It, You Run It.” 
-- (Werner Vogels, CTO of Amazon)