---
title: Cloud cost optimization toolkit
date: 2021-04-07 08:00:00 +0100
categories: [Cloud, Miscellaneous]
tags: [cloud, aws, gcp, azure]
description: Collection of links to tools and articles about cost optimization in public cloud.
---
Looking for knowledge and tools for cloud cost optimization? Here you can find many useful links for Amazon Web Services, Azure and Google Cloud Platform.

## Cost management
Tools for cost monitoring, reporting and alerting.

* AWS
  * [AWS Cost Explorer](https://aws.amazon.com/aws-cost-management/aws-cost-explorer/)

* Azure
  * [Azure Cost Management and Billing](https://azure.microsoft.com/pl-pl/services/cost-management)
  * [Azure Cost Management Tutorial](https://www.youtube.com/watch?v=CXFWPI1qk_s)

* Google Cloud Platform
  * [GCP Cost Management](https://cloud.google.com/cost-management)

## Automatic advisors
Automatic recommendations based on current resuorce consumption.
* AWS
  * [AWS Trusted Advisor](https://aws.amazon.com/premiumsupport/technology/trusted-advisor/)

* Azure
  * [Azure Advisor](https://azure.microsoft.com/en-us/services/advisor/)

* Google Cloud Platform
  * [GCP Recommender](https://cloud.google.com/recommender)

## Instances start/stop scheduling
Automatic startup and shutdown of virtual machines or databases. For example, you can automatically shutdown your development environments for the evening or weekends.
* AWS
    * [AWS Instance Scheduler](https://aws.amazon.com/solutions/implementations/instance-scheduler/) - complex solution from AWS
* Azure
    * [Start/Stop VMs during off-hours](https://docs.microsoft.com/en-us/azure/automation/automation-solution-vm-management)
* Google Cloud Platform
    * [Scheduling compute instances with Cloud Scheduler](https://cloud.google.com/scheduler/docs/start-and-stop-compute-engine-instances-on-a-schedule)
    * [github.com/lmaczulajtys/gcp-instance-scheduler](https://github.com/lmaczulajtys/gcp-instance-scheduler) - similar to AWS solution written in Python
    * [github.com/future-architect/gcp-instance-scheduler](https://github.com/future-architect/gcp-instance-scheduler) - different approach in Go

## Instances reservation
Up to 70% discounts for reserving resources for 1 or 3 years.
* AWS
  * If you know amount and type of machines you want to reserve:
    * [Amazon EC2 Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/)
    * [Amazon RDS Reserved Instances](https://aws.amazon.com/rds/reserved-instances/)
  * If you know how much ($/hour) computing power you are buing:
    * [Savings Plans](https://docs.aws.amazon.com/savingsplans/latest/userguide/what-is-savings-plans.html)
* Azure
  * [Reservations](https://azure.microsoft.com/en-us/reservations/)
* Google Cloud Platform
  * [Committed use discounts](https://cloud.google.com/compute/docs/instances/signing-up-committed-use-discounts)

## Spot/preemtible instances
Up to 70% discounts for using temporary virtual machines.
* AWS
  * [Amazon EC2 Spot Instances](https://aws.amazon.com/ec2/spot/?cards.sort-by=item.additionalFields.startDateTime&cards.sort-order=asc)
* Azure
  * [Azure Spot Virtual Machines](https://azure.microsoft.com/en-us/pricing/spot/)
* Google Cloud Platform
  * [Preemptible Virtual Machines](https://cloud.google.com/preemptible-vms)