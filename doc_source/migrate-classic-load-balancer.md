# Migrate your Classic Load Balancer<a name="migrate-classic-load-balancer"></a>

Elastic Load Balancing provides three types of load balancers: Classic Load Balancers, Application Load Balancers, and Network Load Balancers\. For information about the different features of each load balancer type, see [Comparison of Elastic Load Balancing products](https://aws.amazon.com/elasticloadbalancing/details/#compare)\.

**Migration scenarios**

1. If you have an existing Classic Load Balancer in a VPC, verify that an Application Load Balancer or Network Load Balancer meets your needs and then migrate your Classic Load Balancer to one of these load balancer types\.

1. If you have an existing Classic Load Balancer in EC2\-Classic, verify that an Application Load Balancer or Network Load Balancer meets your need and then migrate your Classic Load Balancer to one of these load balancer types\. Otherwise, migrate to a Classic Load Balancer in a VPC\. You can leave your instances in EC2\-Classic and enable ClassicLink to link the instances to the load balancer VPC, or you can migrate your instances from EC2\-Classic to a VPC\.

**Topics**
+ [Step 1: Create a new load balancer](#migrate-create-load-balancer)
+ [Step 2: Gradually redirect traffic to your new load balancer](#migrate-traffic)
+ [Step 3: Update policies, scripts, and code](#migrate-other-updates)
+ [Step 4: Delete the old load balancer](#migrate-cleanup)

## Step 1: Create a new load balancer<a name="migrate-create-load-balancer"></a>

Create a load balancer with a configuration that is equivalent to the Classic Load Balancer to migrate\. After you have completed the migration process, you can take advantage of the features of your new load balancer\.

To create an Application Load Balancer or Network Load Balancer to replace a Classic Load Balancer in a VPC, use one of the following options:
+ [Option 1: Use the migration wizard in the console](#migration-wizard)
+ [Option 2: Use the load balancer copy utility from github](#load-balancer-copy-utility)
+ [Option 3: Migrate manually to an Application Load Balancer or Network Load Balancer](#migrate-step-by-step)

To create a Classic Load Balancer in a VPC to replace a Classic Load Balancer in EC2\-Classic, use the following option:
+ [Option 4: Migrate manually to a Classic Load Balancer in a VPC](#migrate-step-by-step-classiclink)

### Option 1: Use the migration wizard in the console<a name="migration-wizard"></a>

The migration wizard creates an Application Load Balancer or Network Load Balancer based on the configuration of your Classic Load Balancer in a VPC\. The type of load balancer that is created depends on the configuration of the Classic Load Balancer\.

**Migration wizard release notes**
+ The Classic Load Balancer must be in a VPC\.
+ If the Classic Load Balancer has an HTTP or HTTPS listener, the wizard creates an Application Load Balancer\. If the Classic Load Balancer has a TCP listener, the wizard creates a Network Load Balancer\.
+ If the name of the Classic Load Balancer matches the name of an existing Application Load Balancer or Network Load Balancer, the wizard requires that you specify a different name during migration\.
+ If the Classic Load Balancer has one subnet, the wizard requires that you specify a second subnet when creating an Application Load Balancer\.
+ If the Classic Load Balancer has registered instances in EC2\-Classic, they are not registered with the target group for the new load balancer\.
+ If the Classic Load Balancer has registered instances of the following types, they are not registered with the target group for a Network Load Balancer: C1, CC1, CC2, CG1, CG2, CR1, CS1, G1, G2, HI1, HS1, M1, M2, M3, and T1\.
+ If the Classic Load Balancer has HTTP/HTTPS listeners but uses TCP health checks, the wizard changes them to HTTP health checks\. Then it sets the path to "/" by default when creating the Application Load Balancer\.
+ If the Classic Load Balancer is migrated to a Network Load Balancer, the health check settings are changed to meet the requirements for Network Load Balancers\.
+ If the Classic Load Balancer has multiple HTTPS listeners, the wizard chooses one and uses its certificate and policy\. If there is an HTTPS listener on port 443, the wizard chooses this listener\. If the listener that is chosen uses a custom policy or a policy not supported for Application Load Balancers, the wizard changes to the default security policy\.
+ If the Classic Load Balancer has a secure TCP listener, the Network Load Balancer uses a TCP listener\. But it does not use the certificate or security policy\.
+ If the Classic Load Balancer has multiple listeners, the wizard uses the listener port with the lowest value for the target group port\. Each instance registered with these listeners is registered with the target group on the listener ports for all the listeners\.
+ If the Classic Load Balancer has tags with the `aws` prefix in the tag name, these tags are not added to the new load balancer\.

**To migrate a Classic Load Balancer using the migration wizard**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select your Classic Load Balancer\.

1. On the **Migration** tab, choose **Launch ALB Migration Wizard** or **Launch NLB Migration Wizard**\. The button that is displayed depends on the load balancer type that was selected by the wizard after examining your Classic Load Balancer\.

1. On the **Review** page, verify the configuration options selected by the wizard\. To change an option, choose **Edit**\.

1. When you are finished configuring the new load balancer, choose **Create**\.

### Option 2: Use the load balancer copy utility from github<a name="load-balancer-copy-utility"></a>

This load balancer copy utility is available on GitHub\. For more information, see [Elastic Load Balancing Tools](https://github.com/aws/elastic-load-balancing-tools)\.

### Option 3: Migrate manually to an Application Load Balancer or Network Load Balancer<a name="migrate-step-by-step"></a>

The following information provides general instructions for manually creating a new Application Load Balancer or Network Load Balancer based on an existing Classic Load Balancer in a VPC\. You can migrate using the AWS Management Console, the AWS CLI, or an AWS SDK\. For more information, see [Getting started with Elastic Load Balancing](load-balancer-getting-started.md)\.

1. Create a new load balancer, with the same scheme \(internet\-facing or internal\), subnets, and security groups as the Classic Load Balancer\.

1. Create one target group for your load balancer, with the same health check settings that you have for your Classic Load Balancer\.

1. Do one of the following:
   + If your Classic Load Balancer is attached to an Auto Scaling group, attach your target group to the Auto Scaling group\. This also registers the Auto Scaling instances with the target group\.
   + Register your EC2 instances with your target group\.

1. Create one or more listeners, each with a default rule that forwards requests to the target group\. If you create an HTTPS listener, you can specify the same certificate that you specified for your Classic Load Balancer\. We recommend that you use the default security policy\.

1. If your Classic Load Balancer has tags, review them and add the relevant tags to your new load balancer\.

### Option 4: Migrate manually to a Classic Load Balancer in a VPC<a name="migrate-step-by-step-classiclink"></a>

The following information provides general instructions for manually creating a new Classic Load Balancer in a VPC based on a Classic Load Balancer in EC2\-Classic\. You can migrate using the AWS Management Console, the AWS CLI, or an AWS SDK\. For more information, see [Tutorial: Create a Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html) in the *User Guide for Classic Load Balancers*\.

1. Do one of the following:
   + Enable ClassicLink on your VPC and link your EC2\-Classic instances to the VPC\. For more information, see [Use ClassicLink for an incremental migration](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html#classiclink-migrate) in the *Amazon EC2 User Guide*\.
   + Migrate your EC2 resources, such as instances and security groups, from EC2\-Classic to a VPC\. For more information, see [Migrate your resources to a VPC](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html#full-migrate) in the *Amazon EC2 User Guide*\.

1. Create a new Classic Load Balancer in a VPC\.

1. When you create the load balancer, select the VPC that you prepared \(either the VPC with ClassicLink enabled or the VPC that you used when migrating your instances from EC2\-Classic\)\. Select one subnet from each Availability Zone that contains the instances that you plan to register with the new load balancer\.

1. When you are prompted to assign a security group, if the VPC is enabled with ClassicLink, select the same security group that you specified when you enabled ClassicLink\.

1. When prompted, select the instances to register with the load balancer\.

1. If your old Classic Load Balancer has tags, review them and add the relevant tags to your new Classic Load Balancer\.

## Step 2: Gradually redirect traffic to your new load balancer<a name="migrate-traffic"></a>

After your instances are registered with your new load balancer, you can begin the process of redirecting traffic from the old load balancer to the new load balancer\. This enables you to test your new load balancer while minimizing risk to the availability of your application\.

**To redirect traffic gradually to your new load balancer**

1. Paste the DNS name of your new load balancer into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your application\.

1. Create a new DNS record that associates your domain name with your new load balancer\. If your DNS service supports weighting, specify a weight of 1 in the new DNS record and a weight of 9 in the existing DNS record for your old load balancer\. This directs 10% of the traffic to the new load balancer and 90% of the traffic to the old load balancer\.

1. Monitor your new load balancer to verify that it is receiving traffic and routing requests to your instances\.
**Important**  
The time\-to\-live \(TTL\) in the DNS record is 60 seconds\. This means that any DNS server that resolves your domain name keeps the record information in its cache for 60 seconds, while the changes propagate\. Therefore, these DNS servers can still route traffic to your old load balancer for up to 60 seconds after you complete the previous step\. During propagation, traffic could be directed to either load balancer\.

1. Continue to update the weight of your DNS records until all traffic is directed to your new load balancer\. When you are finished, you can delete the DNS record for your old load balancer\.

## Step 3: Update policies, scripts, and code<a name="migrate-other-updates"></a>

If you migrated your Classic Load Balancer to an Application Load Balancer or Network Load Balancer, be sure to do the following:
+ Update IAM policies that use API version 2012\-06\-01 to use version 2015\-12\-01\.
+ Update processes that use CloudWatch metrics in the `AWS/ELB` namespace to use metrics from the `AWS/ApplicationELB` or `AWS/NetworkELB` namespace\.
+ Update scripts that use aws elb AWS CLI commands to use aws elbv2 AWS CLI commands\.
+ Update AWS CloudFormation templates that use the `AWS::ElasticLoadBalancing::LoadBalancer` resource to use the `AWS::ElasticLoadBalancingV2` resources\.
+ Update code that uses Elastic Load Balancing API version 2012\-06\-01 to use version 2015\-12\-01\.

**Resources**
+ [elbv2](https://docs.aws.amazon.com/cli/latest/reference/elbv2/index.html) in the *AWS CLI Command Reference*
+ [Elastic Load Balancing API Reference version 2015\-12\-01](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)
+ [Identity and access management for Elastic Load Balancing](load-balancer-authentication-access-control.md)
+ [Application Load Balancer metrics](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html#load-balancer-metrics-alb) in the *User Guide for Application Load Balancers*
+ [Network Load Balancer metrics](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html#load-balancer-metrics-nlb) in the *User Guide for Network Load Balancers*
+ [AWS::ElasticLoadBalancingV2::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-loadbalancer.html) in the *AWS CloudFormation User Guide*

## Step 4: Delete the old load balancer<a name="migrate-cleanup"></a>

You can delete the old Classic Load Balancer after:
+ You have redirected all traffic from the old load balancer to the new load balancer\.
+ All existing requests that were routed to the old load balancer have completed\.