---
layout: post
title:  "Tsuru PaaS"
author: "Jean-Marie Joly"
date:   2017-10-17
thumbnail: /img/posts/tsuru/tsuru.png
categories: [development]
tags: [devops]
permalink: /tsuru-paas/
---

# Tsuru PaaS
<center>
	<img alt="Tsuru logo" src="/img/posts/tsuru/tsuru.png" style="width: 90%; margin-bottom: 10px;" />
</center>  


This article is about how a Platform as a Service, named Tsuru, allowed us to
handle an increasing flow of applications, widely scale our infrastructure and
reduce both costs and workload caused by our growth. With a short learning
curve, Tsuru set a high and reproducible quality in production.

## Scaling problem

Let’s say you are a new systems engineer at Jumia Travel in charge of site reliability. What would you do if you had to run in production 10 new applications every month? every week? every day? Would you cope with that amount of change without degrading production quality or jeopardizing your budget? A Platform as a Service (PaaS) may be an adequate solution, however finding the right trade-off in terms of cost, feature and learning curve is not trivial.

"Production-ready" is not just a matter of having your application running and
reliable in a production environment. "Production-ready" implies you have
defined processes and tools to deploy, run, monitor and change your application according to real
world use-cases. For instance, for each new service you implement, you probably
need at least the following:

  * continuous integration/delivery pipeline;
  * deployment process (e.g. continuous deployment);
  * system configuration management recipe;
  * system/middleware/application monitoring;
  * system/middleware/application logging;
  * high availability;
  * horizontal scaling or auto-scaling.

Implementing all these is a lot of work you may not have planned up
front... Worse, you may even have to adapt those components according to the type of
service - starting with the programming language. Overall, it won't scale for a
increasing flow of new services you wish to run in production.

## Solution: increase service level

The table below compares service levels in cloud computing. It was the starting point to solve our scaling problem.

<center>
	<img alt="Cloud service levels" src="/img/posts/tsuru/level.png" style="width: 90%; margin-bottom: 10px;" />
</center>  

Most cloud-based companies now fully operate on IaaS. But, as you can see in the comparison table, IaaS misses a lot of automation in terms of service level that could be beneficial to scale. PaaS is the next and obvious step after IaaS to
abstract away the underlying infrastructure so you can focus solely on your application.

You may be considering CaaS (Container as a Service) as a halfway solution between IaaS and PaaS. CaaS is merely a container-based IaaS enhanced with richer features. However, container-based PaaS usually includes CaaS capabilities, so it makes sense to go up to the PaaS level.

After reviewing PaaS solutions, we noticed that most of them support the previous list of services surrounding production environment (and not only production). But which PaaS should I get?

To choose a PaaS, you must decide first if you go for a public or a private one. To keep it simple, public means the provider manages the PaaS, private means you're in charge. A public PaaS sounds nice, but it obviously implies a higher cost. In the end, we went for a private PaaS as we had to be sensible about our budget. However, since we would manage our own system, the PaaS solution had to be simple, with a short learning curve, and it had to also include all the features a modern PaaS should have. For these reasons we chose Tsuru.

## Why tsuru?
Because it's a simple and complete solution. Tsuru PaaS is what we installed and configured in our infrastructure to radically scale service integration to production at Jumia Travel, and we got started in a matter of minutes! Besides, it's free and open source. So let's dig a little into Tsuru.

### Platforms

Tsuru supports many platforms, check this [list](https://github.com/tsuru/platforms), you'll find most of the popular programming languages. Note that you can also instantiate backing services like databases (e.g. MySQL, MongoDB) along with your application. If you're not yet satisfied with that current list you can either use Heroku buildpacks or easily create your own custom platform with a simple Dockerfile.

### Workflow

Tsuru workflow is very straight forward. A single git push triggers the entire
workflow to build, release and deploy application changes for users to enjoy.
Have a look at the following figure:

<center>
	<img alt="Tsuru workflow" src="/img/posts/tsuru/workflow.png" style="width: 90%; margin-bottom: 10px;" />
</center>  

### IaaS/CaaS

Probably the most remarkable thing about Tsuru is that it aims to be *technology
agnostic*, so that it can run on top of most current IaaS solutions and use
different CaaS technologies. In our case we used AWS EC2 and Docker Engine, this
means Tsuru's own scheduler manages Docker containers. Other options for
container orchestration are Docker Swarm (Tsuru 1.2) and Kubernetes (Tsuru 1.3).
Even though Swarm or Kubernetes increase service level to CaaS, you can still
benefit from Tsuru PaaS for higher level tasks (such as application deployment)
that are not easily performed by CaaS.

### Horizontal scaling

With Tsuru you can scale horizontally twice: at the host level (Docker node) and at the guest level (Docker container). This means each piece of application scales independently while the entire cluster scales according to the overall demand. This makes Tsuru extremely flexible and scalable.

<center>
	<img alt="Tsuru auto-scaling" src="/img/posts/tsuru/auto-scaling.png" style="width: 90%; margin-bottom: 10px;" />
</center>  


## Let's get started!

To test the waters of Tsuru, we decided to begin by only migrating our
increasing number of new, private microservices. By postponing the migration of
our public applications and databases we could minimize the customer impact in
the event of a Paas failure. Besides, we knew we would get into trouble to go to production
on time, with very little budget allocated for each service's infrastructure, due
to service size.

*Today, all of Jumia Travel's private microservices run steadily on Tsuru, and
until now we haven't experienced any service failures.* Performance-wise we have
lost less than 5% application response time (primarily due to Docker
virtualization overhead) but have gained many benefits that make up for it.

Jumia infrastructure is mainly hosted in AWS. For this reason, we tried to
integrate as much as possible with AWS services, and the next sections reflect
this strategy. Note that Tsuru integrates very well with other cloud providers
that have similar services.

### Tsuru components
Tsuru core components are written in Go. These are the main components and services:

  * Tsuru server
  * Gandalf (git server)
  * Archive server (git archive)
  * PlanB router
  * MongoDB
  * Redis
  * Docker Registry
  * Docker Engine

What makes Tsuru highly scalable is that each component of this list can scale
independently of the others. For instance, Tsuru core components and Docker
Engine naturally scale horizontally; Redis and MongoDB support master/slave
replication, and you can directly use AWS Elastic Container Registry as a
Docker registry to manage your application images. With Tsuru you can start
with a very small infrastructure - in AWS, all Tsuru components can run on a
t2.micro instance (1 CPU, 1 GB of RAM) for test or development purposes.
You may then progressively scale each component according to its usage.

As we began managing more services with Tsuru we noticed that Docker Engine
and then PlanB router consume most of the allocated resources. This is the
reason why they should be the first components to scale. The figure below
shows the overall infrastructure.


<center>
	<img alt="Tsuru infra" src="/img/posts/tsuru/infra.png" style="width: 90%; margin-bottom: 10px;" />
</center>  


AWS auto-scaling can directly provision and retire PlanB routers, whereas
Docker nodes require Tsuru auto-scaling integration into AWS. This is critical
for service delivery as Docker nodes aren't aware of container health, whereas
Tsuru is. This why Tsuru itself handles auto-scaling to safely provision and
retire Docker nodes, while initiating or draining container connections from
PlanB routers via Redis.

### Logging

Best practice for logging in containers is to redirect any log to *stdout* and
*stderr*. This way, Tsuru captures container logs that may be freely
processed.  However, if you use AWS instance roles you may actually configure
Docker (and Tsuru) to send all logs directly to Cloudwatch. This gives you a
nice interface to browse your application logs.

```
--log-driver awslogs --log-opt awslogs-region=<aws-region-code> \
--log-opt awslogs-group=my.tsuru.cluster --log-opt tag='{% raw %}{{.Name}}{% endraw %}'
```

### Service discovery

Service discovery is a complex problem that Tsuru solves for
you: it uses the platform, Planb router and Redis to manage routes and
traffic. When you create an application in Tsuru, you get two endpoints: a Git
endpoint to push your application code and a second endpoint for HTTP access.
As deployment occurs, Tsuru creates a new container and waits until it's
healthy to retire the old one. This way, users seamlessly switch to the new
version of the application. In the event that the container never becomes healthy, Tsuru
aborts the deployment. Tsuru healthcheck is customizable for each application
in a *tsuru.yaml* file:

```
healthcheck:
    path: /healthcheck
    method: GET
    status: 200
    match: .*OK.*
    allowed_failures: 3
```

### 12 Factor App

As we were implementing Tsuru, we realized it helped us reach most, *if not
all*, of the [12 factor app](https://12factor.net/) principles that make
cloud-based software more manageable, portable and scalable. Have a look at
those best practices if you don't know them yet, there are a cornerstone for
applications running on PaaS, SaaS and even CaaS.

### Give it a try

Want to test Tsuru? Download Tsuru [Now](https://github.com/tsuru/now) follow this simple video [example](https://www.youtube.com/watch?v=xgX_gwXj3Js) to create, customize and scale a hello world PHP application in Tsuru.

## Some references
Check out the following links for additional information:

  - [Tsuru PaaS at TechInPorto conference](https://www.youtube.com/watch?v=GiMsS0vGmn4)
  - [Tsuru blog](https://blog.tsuru.io/)
  - [Tsuru official page](https://tsuru.io)
