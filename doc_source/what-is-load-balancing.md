# What Is Elastic Load Balancing?<a name="what-is-load-balancing"></a>

Elastic Load Balancing distributes incoming application or network traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in multiple Availability Zones\. Elastic Load Balancing scales your load balancer as traffic to your application changes over time, and can scale to the vast majority of workloads automatically\.

## Load Balancer Benefits<a name="load-balancer-benefits"></a>

A load balancer distributes workloads across multiple compute resources, such as virtual servers\. Using a load balancer increases the availability and fault tolerance of your applications\.

You can add and remove compute resources from your load balancer as your needs change, without disrupting the overall flow of requests to your applications\.

You can configure health checks, which are used to monitor the health of the compute resources so that the load balancer can send requests only to the healthy ones\. You can also offload the work of encryption and decryption to your load balancer so that your compute resources can focus on their main work\.

## Features of Elastic Load Balancing<a name="elb-features"></a>

Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. You can select a load balancer based on your application needs\. For more information, see [Comparison of Elastic Load Balancing Products](https://aws.amazon.com/elasticloadbalancing/details/#compare)\.

For more information about using each load balancer, see the [User Guide for Application Load Balancers](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/), the [User Guide for Network Load Balancers](http://docs.aws.amazon.com/elasticloadbalancing/latest/network/), and the [User Guide for Classic Load Balancers](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)\.

## Accessing Elastic Load Balancing<a name="elb-access-methods"></a>

You can create, access, and manage your load balancers using any of the following interfaces:
+ **AWS Management Console**— Provides a web interface that you can use to access Elastic Load Balancing\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Elastic Load Balancing, and is supported on Windows, Mac, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com//cli/)\.
+ **AWS SDKs** — Provides language\-specific APIs and takes care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](http://aws.amazon.com/tools/#SDKs)\.
+ **Query API**— Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Elastic Load Balancing, but it requires that your application handle low\-level details such as generating the hash to sign the request, and error handling\. For more information, see the following:
  + Application Load Balancers and Network Load Balancers — [API version 2015\-12\-01](http://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)
  + Classic Load Balancers — [API version 2012\-06\-01](http://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/)

## Related Services<a name="elb-related-services"></a>

Elastic Load Balancing works with the following services to improve the availability and scalability of your applications\.
+ **Amazon EC2** — Virtual servers that run your applications in the cloud\. You can configure your load balancer to route traffic to your EC2 instances\. For more information, see the [Amazon EC2 User Guide for Linux Instances](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) or the [Amazon EC2 User Guide for Windows Instances](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/)\.
+ **Amazon ECS** — Enables you to run, stop, and manage Docker containers on a cluster of EC2 instances\. You can configure your load balancer to route traffic to your containers\. For more information, see the [Amazon Elastic Container Service Developer Guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\.
+ **Auto Scaling** — Ensures that you are running your desired number of instances, even if an instance fails, and enables you to automatically increase or decrease the number of instances as the demand on your instances changes\. If you enable Auto Scaling with Elastic Load Balancing, instances that are launched by Auto Scaling are automatically registered with the load balancer, and instances that are terminated by Auto Scaling are automatically de\-registered from the load balancer\. For more information, see the [Amazon EC2 Auto Scaling User Guide](http://docs.aws.amazon.com/autoscaling/latest/userguide/)\.
+ **Amazon CloudWatch** — Enables you to monitor your load balancer and take action as needed\. For more information, see the [Amazon CloudWatch User Guide](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.
+ **Route 53** — Provides a reliable and cost\-effective way to route visitors to websites by translating domain names \(such as `www.example.com`\) into the numeric IP addresses \(such as `192.0.2.1`\) that computers use to connect to each other\. AWS assigns URLs to your resources, such as load balancers\. However, you might want a URL that is easy for users to remember\. For example, you can map your domain name to a load balancer\. For more information, see the [Amazon Route 53 Developer Guide](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/)\.

## Pricing<a name="load-balancer-pricing"></a>

With your load balancer, you pay only for what you use\. For more information, see [Elastic Load Balancing Pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\.