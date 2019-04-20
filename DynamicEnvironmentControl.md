# Dynamic Environment Control 
## Objective
Team needs multiple environments to conduct development work, quality assessment, assurance and control, conducting demo to customers and a live environment. While the live environment remains mostly as is, unless a change is desired, the other environments can be spinned on and off based on need. Keeping them up and running is expensive and not having them is obstructing for business. Need a mechanism to shut down services when they are not in use and business is being billed for not using them.

* Services billed based on usage pattern need not be controlled using this approach.
* Services billed based on uptime will be controlled using this capability.
* Services billed based on volume will be controlled using this capability.

## Services in consideration
* EC2: Runs the following processes
  * Redis
  * MySQL
* S3
  * Regular S3
  * S3-IA
  * S3-RRS
  * S3-Glacier
* SNS
* SQS
* ECR
* ECS



## Scope for POC

### Deployment Structure
* 2 EC2 instance running some microservices across two AZ
* 2 EC2 instance running a web application across two AZ
* 1 Load Balancer fronting the two microservices
* 1 Load Balancer fronting the web application
* 1 EC2 hosting MySQL
* 1 EC2 hosting Redis
* Images stored in ECR
* Instances are deployed in ECS 

### Option : Pricing Model
Change in pricing model for ECS. Migrate from EC2 Launch Type Model to Fargate Launch Type Model

EC2 Launch Type Pricing Model
* Pay for AWS resources (e.g. EC2 instances or EBS volumes) that are created by the container. 
* There are no upfront fees or discounts. 
* Charged based on uptime and volume (number of EC2 consumed, volume of EBS); not on usage (CPU utillization, etc.)
* If environment is not used, resources are still allocated and billed.

Fargate Launch Type Model
* Pay for the amount of vCPU and memory resources that containerized application requests
* If environment is provisioned and not used, there are none to minimal charges

Benefits:
* No significant redesign or rearchitecture involved
* Seamless transformation to new pricing model

Concerns:
* Need to evaluate on peak usage, low usage and average usage, what is the cost difference across the two models. 
* Whether Fargate pricing model benefits only on low usage volume, or it is beneficial in peak as well?

Additional discovery needed:
* Should production be also moved to Fargate pricing model?
* How much will this be beneficial if Redis moves to ElastiCache and MySQL moves to RDS and the microservices moves to serverless (lambda)
* EKS does not support Fargate pricing model. Is it a roadmap of having containers moved from ECS to EKS. 

