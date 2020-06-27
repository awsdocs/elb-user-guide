# Resilience in Elastic Load Balancing<a name="disaster-recovery-resiliency"></a>

The AWS global infrastructure is built around AWS Regions and Availability Zones\. Regions provide multiple physically separated and isolated Availability Zones, which are connected through low\-latency, high\-throughput, and highly redundant networking\. With Availability Zones, you can design and operate applications and databases that automatically fail over between zones without interruption\. Availability Zones are more highly available, fault tolerant, and scalable than traditional single or multiple data center infrastructures\.

For more information about AWS Regions and Availability Zones, see [AWS Global Infrastructure](http://aws.amazon.com/about-aws/global-infrastructure/)\.

In addition to the AWS Global Infrastructure, Elastic Load Balancing provides the following features to support your data resiliency:
+ Distributes incoming traffic across multiple instances in a single Availability Zone or multiple Availability Zones\.
+ You can use AWS Global Accelerator with your Application Load Balancers to distribute incoming traffic across multiple load balancers in one or more AWS Regions\. For more information, see the [AWS Global Accelerator Developer Guide](https://docs.aws.amazon.com/global-accelerator/latest/dg/)\.
+ Amazon ECS enables you to run, stop, and manage Docker containers on a cluster of EC2 instances\. You can configure your Amazon ECS service to use a load balancer to distribute incoming traffic across the services in a cluster\. For more information, see the [Amazon Elastic Container Service Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\.