---
layout: post
title:  "Immutable Infrastructure & GitOps"
author: "Karim Abo El-Azm"
date:   2022-09-21
thumbnail: /img/posts/immutable-infrastructure-gitops/immutable-infra-gitops-main.png
categories: [development]
tags: [devops, sre, gitops]
permalink: /immutable-infrastructure-gitops/
---

<center>
    <img alt="Immutable Infrastructure & GitOps" src="/img/posts/immutable-infrastructure-gitops/immutable-infra-gitops-main.png" style="width: 90%; margin-bottom: 10px;" />
</center>

# Introduction

**Mutable Infrastructure**: Mutable simply means 'changeable' or 'customizable'. This means that you can login into the server and update configurations in place. Engineers and administrators working with this kind of infrastructure can SSH into their servers, upgrade or downgrade packages manually, tweak configuration files on a server-by-server basis, and deploy new code directly onto existing servers.

**Immutable Infrastructure**: Servers are never modified after they're deployed. If something needs to be updated, fixed, or modified in any way, new servers built from a common image with the appropriate changes are provisioned to replace the old ones. After they're validated, they're put into use and the old ones are decommissioned.

<center>
    <img alt="Immutable and Mutable Infra compared" src="/img/posts/immutable-infrastructure-gitops/mutable-immutable-compared.png" style="width: 90%; margin-bottom: 10px;" />
</center>

## Benefits and Limitations (Mutable Infrastructure)

Benefits:

1. Totally customizable and mutable = Precisely fits the application specific needs
2. No need to deploy new servers with new changes
3. Faster updates

Limitations:

1. Manual changes = More error prone + More time consuming
2. Possibility of the existence of undocumented changes = Inability of version tracking and debugging
3. Servers have inconsistent configurations = Configuration Drift
4. Harder to troubleshoot/reproduce errors
5. More risks during update/upgrade

## Benefits and Limitations (Immutable Infrastructure)

Benefits:

1. No room for configuration drift = Easier testing
2. More consistency and reliability in infrastructure = Higher up-time
3. Simpler and more predictable deployment process = Lower risks during update/upgrade
4. Discrete Versioning and Easier Tracking
5. Easier rollback to older versions

Limitations:

1. Infrastructure can't be modified in place
2. Necessity of externalizing data

## Why does DevOps use Immutable Infrastructure?

Immutable infrastructure is used most by DevOps. It is affordable and easy to create new servers using modern DevOps. If there is any room for updates or improvements in immutable infrastructure, you need to replace the whole server.

Immutable infrastructure is strongly correlated with the concept of infrastructure as a code. Infrastructure as a code allows you to do all the planning of the components like instances, networking, and security. And then you can push it into your dev environment. As you promote those to the dev environment, test and prod, it is quite easy to repeat these steps in a consistent manner with immutable infrastructure. This will also make sure that no matter what environment the app developers might be in, there will always be a consistent environment. They don't need to worry about it while deploying applications.

For all these reasons, immutable infrastructure is highly associated with the DevOps practice. DevOps includes culture and tools that work to achieve agile development with continuous delivery. And continuous delivery drives immutable infrastructure.

### Immutable Infrastructure Tools
There are plenty of tools that leverage the concept of Immutable Infrastructure. These tools include:
1. Packer
2. Terraform
3. Docker/Kubernetes

**Packer**: free and open source tool for creating machine images for multiple platforms from a single source configuration. A machine image is a single static unit that contains a pre-configured operating system and installed software which is used to quickly create new running machines. Machine image formats change for each platform.

**Terraform**: infrastructure as code tool that lets you define both cloud and on-prem resources in human-readable configuration files that you can version, reuse, and share. You can then use a consistent workflow to provision and manage all of your infrastructure throughout its lifecycle. Terraform can manage low-level components like compute, storage, and networking resources, as well as high-level components like DNS entries and SaaS features.

**Docker/Kubernetes**: containerizing tools that help provision your infrastructure in the form of containers instead of VMs. This helps more in enabling an immutable infrastructure as creating/destroying containers is much easier and faster than spanning new VMs.

What we have been using at Jumia:

1. Packer to build our AWS AMIs that we use for our EC2 instances and EKS nodes as a base OS. These AMIs contain the initial steps and data to bootstrap our servers.
2. Terraform to build our AWS infrastructure like EC2 instances, RDS, Route53 records, and so on. This helps us avoid any manual changes to our infrastructure, and makes it documented.
3. EKS to build our AWS Kubernetes clusters to host our applications for different environments and verticals. This helps us more in managing our applications lifecycle and debugging/troubleshooting them in case a problem occurs.

## GitOps

<center>
    <img alt="GitOps" src="/img/posts/immutable-infrastructure-gitops/gitops.png" style="width: 90%; margin-bottom: 10px;" />
</center>

GitOps is code-based infrastructure and operational procedures that rely on Git as a source control system. It's an evolution of Infrastructure as Code (IaC) and a DevOps best practice that leverages Git as the single source of truth, and control mechanism for creating, updating, and deleting system architecture. More simply, it is the practice of using Git pull requests to verify and automatically deploy system infrastructure modifications.

GitOps takes full advantage of the move towards immutable infrastructure and declarative container orchestration. In order to minimize the risk of change after a deployment, whether intended or by accident via "configuration drift" it is essential that we maintain a reproducible and reliable deployment process.

GitOps ensures that a system's cloud infrastructure is immediately reproducible based on the state of a Git repository. Pull requests modify the state of the Git repository. Once approved and merged, the pull requests will automatically reconfigure and sync the live infrastructure to the state of the repository. This live syncing pull request workflow is the core essence of GitOps.

To achieve a full GitOps install, a pipeline platform is required. Pipelines automate and bridge the gap between Git pull requests and the orchestration system. Once pipeline hooks are established and triggered from pull requests, commands are executed to the orchestration piece.
The GitOps methodology has several advantages:
- Single source of truth. All code is on Git, so there is a single source of truth for the infrastructure or platform.
- Easy rollback and fast recovery. All changes to the infrastructure are version controlled and reviewed so they are easy to roll back if necessary.
- Enhanced security. All changes are pulled and applied by automation; this limits manual access to the infrastructure and thus improves security.

<center>
    <img alt="GitOps Process" src="/img/posts/immutable-infrastructure-gitops/gitops-process.png" style="width: 90%; margin-bottom: 10px;" />
</center>

What we have been using at Jumia:

1. Atlantis to migrate from old self-managed Kubernetes clusters to new AWS EKS clusters. Atlantis makes use of Terraform to compare the infrastructure state of the current infrastructure and the infrastructure changes on Github. It can then show the differences and apply them if the user approves them.
