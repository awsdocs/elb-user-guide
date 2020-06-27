# What is Elastic Load Balancing?<a name="what-is-load-balancing"></a>

Elastic Load Balancing distributes incoming application or network traffic across multiple targets, such as Amazon EC2 instances, containers, and IP addresses, in multiple Availability Zones\. Elastic Load Balancing scales your load balancer as traffic to your application changes over time\. It can automatically scale to the vast majority of workloads\.

## Load balancer benefits<a name="load-balancer-benefits"></a>

A load balancer distributes workloads across multiple compute resources, such as virtual servers\. Using a load balancer increases the availability and fault tolerance of your applications\.

You can add and remove compute resources from your load balancer as your needs change, without disrupting the overall flow of requests to your applications\.

You can configure health checks, which monitor the health of the compute resources, so that the load balancer sends requests only to the healthy ones\. You can also offload the work of encryption and decryption to your load balancer so that your compute resources can focus on their main work\.

## Features of Elastic Load Balancing<a name="elb-features"></a>

Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. You can select a load balancer based on your application needs\. For more information, see [Product comparisons](https://aws.amazon.com/elasticloadbalancing/details/#compare) for Elastic Load Balancing\.

For more information about using each load balancer, see the [User Guide for Application Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/), the [User Guide for Network Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/), and the [User Guide for Classic Load Balancers](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/)\.

## Accessing Elastic Load Balancing<a name="elb-access-methods"></a>

You can create, access, and manage your load balancers using any of the following interfaces:
+ **AWS Management Console**— Provides a web interface that you can use to access Elastic Load Balancing\.
+ **AWS Command Line Interface \(AWS CLI\)** — Provides commands for a broad set of AWS services, including Elastic Load Balancing\. The AWS CLI is supported on Windows, macOS, and Linux\. For more information, see [AWS Command Line Interface](https://aws.amazon.com/cli/)\.
+ **AWS SDKs** — Provide language\-specific APIs and take care of many of the connection details, such as calculating signatures, handling request retries, and error handling\. For more information, see [AWS SDKs](https://aws.amazon.com/tools/#SDKs)\.
+ **Query API**— Provides low\-level API actions that you call using HTTPS requests\. Using the Query API is the most direct way to access Elastic Load Balancing\. However, the Query API requires that your application handle low\-level details such as generating the hash to sign the request, and error handling\. For more information, see the following:
  + Application Load Balancers and Network Load Balancers — [API version 2015\-12\-01](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)
  + Classic Load Balancers — [API version 2012\-06\-01](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/)

## Related services<a name="elb-related-services"></a>

Elastic Load Balancing works with the following services to improve the availability and scalability of your applications\.
+ **Amazon EC2** — Virtual servers that run your applications in the cloud\. You can configure your load balancer to route traffic to your EC2 instances\. For more information, see the [Amazon EC2 User Guide for Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/) or the [Amazon EC2 User Guide for Windows Instances](https://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/)\.
+ **Amazon EC2 Auto Scaling** — Ensures that you are running your desired number of instances, even if an instance fails\. Amazon EC2 Auto Scaling also enables you to automatically increase or decrease the number of instances as the demand on your instances changes\. If you enable Auto Scaling with Elastic Load Balancing, instances that are launched by Auto Scaling are automatically registered with the load balancer\. Likewise, instances that are terminated by Auto Scaling are automatically de\-registered from the load balancer\. For more information, see the [Amazon EC2 Auto Scaling User Guide](https://docs.aws.amazon.com/autoscaling/latest/userguide/)\.
+ **AWS Certificate Manager** — When you create an HTTPS listener, you can specify certificates provided by ACM\. The load balancer uses certificates to terminate connections and decrypt requests from clients\.
+ **Amazon CloudWatch** — Enables you to monitor your load balancer and to take action as needed\. For more information, see the [Amazon CloudWatch User Guide](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/)\.
+ **Amazon ECS** — Enables you to run, stop, and manage Docker containers on a cluster of EC2 instances\. You can configure your load balancer to route traffic to your containers\. For more information, see the [Amazon Elastic Container Service Developer Guide](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/)\.
+ **AWS Global Accelerator** — Improves the availability and performance of your application\. Use an accelerator to distribute traffic across multiple load balancers in one or more AWS Regions\. For more information, see the [AWS Global Accelerator Developer Guide](https://docs.aws.amazon.com/global-accelerator/latest/dg/)\.
+ **Route 53** — Provides a reliable and cost\-effective way to route visitors to websites by translating domain names into the numeric IP addresses that computers use to connect to each other\. For example, it would translate `www.example.com` into the numeric IP address `192.0.2.1`\. AWS assigns URLs to your resources, such as load balancers\. However, you might want a URL that is easy for users to remember\. For example, you can map your domain name to a load balancer\. For more information, see the [Amazon Route 53 Developer Guide](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/)\.
+ **AWS WAF** — You can use AWS WAF with your Application Load Balancer to allow or block requests based on the rules in a web access control list \(web ACL\)\. For more information, see the [AWS WAF Developer Guide](https://docs.aws.amazon.com/waf/latest/developerguide/)\.

## Pricing<a name="load-balancer-pricing"></a>

With your load balancer, you pay only for what you use\. For more information, see [Elastic Load Balancing pricing](https://aws.amazon.com/elasticloadbalancing/pricing/)\.