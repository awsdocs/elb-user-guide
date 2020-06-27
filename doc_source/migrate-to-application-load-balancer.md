# Migrate your Classic Load Balancer<a name="migrate-to-application-load-balancer"></a>

If you have an existing Classic Load Balancer in a VPC, and you have determined that an Application Load Balancer or a Network Load Balancer would meet your needs, you can migrate your Classic Load Balancer\. After you have completed the migration process, you can take advantage of the features of your new load balancer\. For more information, see [Comparison of Elastic Load Balancing products](https://aws.amazon.com/elasticloadbalancing/details/#compare)\.

**Topics**
+ [Step 1: Create a new load balancer](#migrate-create-load-balancer)
+ [Step 2: Gradually redirect traffic to your new load balancer](#migrate-traffic)
+ [Step 3: Update references to your Classic Load Balancer](#migrate-other-updates)
+ [Step 4: Delete the Classic Load Balancer](#migrate-cleanup)

## Step 1: Create a new load balancer<a name="migrate-create-load-balancer"></a>

Create an Application Load Balancer or a Network Load Balancer with a configuration that is equivalent to your Classic Load Balancer\.

You can create the load balancer and target group using one of the following methods:
+ [Migration wizard in the console](#migration-wizard)
+ [Load Balancer Copy Utility](#load-balancer-copy-utility)
+ [Manually](#migrate-step-by-step)

### Option 1: Migrate using the migration wizard<a name="migration-wizard"></a>

The migration wizard creates an Application Load Balancer or Network Load Balancer based on the configuration of your Classic Load Balancer\. The type of load balancer that is created depends on the configuration of the Classic Load Balancer\.

**Migration wizard release notes**
+ The Classic Load Balancer must be in a VPC\.
+ If the Classic Load Balancer has an HTTP or HTTPS listener, the wizard can create an Application Load Balancer\. If the Classic Load Balancer has a TCP listener, the wizard can create a Network Load Balancer\.
+ If the name of the Classic Load Balancer matches the name of an existing Application Load Balancer or Network Load Balancer, the wizard requires that you specify a different name during migration\.
+ If the Classic Load Balancer has one subnet, the wizard requires that you specify a second subnet when creating an Application Load Balancer\.
+ If the Classic Load Balancer has registered instances in EC2\-Classic, they are not registered with the target group for the new load balancer\.
+ If the Classic Load Balancer has registered instances of the following types, they are not registered with the target group for a Network Load Balancer: C1, CC1, CC2, CG1, CG2, CR1, CS1, G1, G2, HI1, HS1, M1, M2, M3, and T1\.
+ If the Classic Load Balancer has HTTP/HTTPS listeners but uses TCP health checks, the wizard changes to HTTP health checks\. Then it sets the path to "/" by default when creating an Application Load Balancer\.
+ If the Classic Load Balancer is migrated to a Network Load Balancer, the health check settings are changed to meet the requirements for Network Load Balancers\.
+ If the Classic Load Balancer has multiple HTTPS listeners, the wizard chooses one and uses its certificate and policy\. If there is an HTTPS listener on port 443, the wizard chooses this listener\. If the listener that is chosen uses a custom policy or a policy not supported for Application Load Balancers, the wizard changes to the default security policy\.
+ If the Classic Load Balancer has a secure TCP listener, the Network Load Balancer uses a TCP listener\. But it does not use the certificate or security policy\.
+ If the Classic Load Balancer has multiple listeners, the wizard uses the listener port with the lowest value as the target group port\. Each instance registered with these listeners is registered with the target group on the listener ports for all the listeners\.
+ If the Classic Load Balancer has tags with the `aws` prefix in the tag name, these tags are not added to the new load balancer\.

**To migrate a Classic Load Balancer using the migration wizard**

1. Open the Amazon EC2 console at [https://console\.aws\.amazon\.com/ec2/](https://console.aws.amazon.com/ec2/)\.

1. On the navigation pane, under **LOAD BALANCING**, choose **Load Balancers**\.

1. Select your Classic Load Balancer\.

1. On the **Migration** tab, choose **Launch ALB Migration Wizard** or **Launch NLB Migration Wizard**\. The button that is displayed depends on the load balancer type that was selected by the wizard after examining your Classic Load Balancer\.

1. On the **Review** page, verify the configuration options selected by the wizard\. To change an option, choose **Edit**\.

1. When you are finished configuring the new load balancer, choose **Create**\.

### Option 2: Migrate using the load balancer copy utility<a name="load-balancer-copy-utility"></a>

This utility is available on GitHub\. For more information, see [Load balancer copy utility](https://github.com/aws/elastic-load-balancing-tools)\.

### Option 3: Migrate manually<a name="migrate-step-by-step"></a>

The following information provides general instructions for manually creating a new load balancer based on a Classic Load Balancer\. You can migrate using the AWS Management Console, the AWS CLI, or an AWS SDK\. For more information, see [Getting started with Elastic Load Balancing](load-balancer-getting-started.md)\.
+ Create a new load balancer, with the same scheme \(internet\-facing or internal\), subnets, and security groups as the Classic Load Balancer\.
+ Create one target group for your load balancer, with the same health check settings that you have for your Classic Load Balancer\.
+ Do one of the following:
  + If your Classic Load Balancer is attached to an Auto Scaling group, attach your target group to the Auto Scaling group\. This also registers the Auto Scaling instances with the target group\.
  + Register your EC2 instances with your target group\.
+ Create one or more listeners, each with a default rule that forwards requests to the target group\. If you create an HTTPS listener, you can specify the same certificate that you specified for your Classic Load Balancer\. We recommend that you use the default security policy\.
+ If your Classic Load Balancer has tags, review them and add the relevant tags to your new load balancer\.

## Step 2: Gradually redirect traffic to your new load balancer<a name="migrate-traffic"></a>

After your instances are registered with your new load balancer, you can begin the process of redirecting traffic to it\. This allows you to test your new load balancer\.

**To redirect traffic gradually to your new load balancer**

1. Paste the DNS name of your new load balancer into the address field of an internet\-connected web browser\. If everything is working, the browser displays the default page of your server\.

1. Create a new DNS record that associates your domain name with your new load balancer\. If your DNS service supports weighting, specify a weight of 1 in the new DNS record and a weight of 9 in the existing DNS record for your Classic Load Balancer\. This directs 10% of the traffic to the new load balancer and 90% of the traffic to the Classic Load Balancer\.

1. Monitor your new load balancer to verify that it is receiving traffic and routing requests to your instances\.
**Important**  
The time\-to\-live \(TTL\) in the DNS record is 60 seconds\. This means that any DNS server that resolves your domain name keeps the record information in its cache for 60 seconds, while the changes propagate\. Therefore, these DNS servers can still route traffic to your Classic Load Balancer for up to 60 seconds after you complete the previous step\. During propagation, traffic could be directed to either load balancer\.

1. Continue to update the weight of your DNS records until all traffic is directed to your new load balancer\. When you are finished, you can delete the DNS record for your Classic Load Balancer\.

## Step 3: Update references to your Classic Load Balancer<a name="migrate-other-updates"></a>

Now that you have migrated your Classic Load Balancer, be sure to update any references to it, such as the following:
+ Scripts that use the AWS CLI aws elb commands \(instead of the aws elbv2 commands\)
+ Code that uses Elastic Load Balancing API version 2012\-06\-01 \(instead of version 2015\-12\-01\)
+ IAM policies that use API version 2012\-06\-01 \(instead of version 2015\-12\-01\)
+ Processes that use CloudWatch metrics
+ AWS CloudFormation templates

**Resources**
+ [elbv2](https://docs.aws.amazon.com/cli/latest/reference/elbv2/index.html) in the *AWS CLI Command Reference*
+ [Elastic Load Balancing API Reference version 2015\-12\-01](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)
+ [Identity and access management for Elastic Load Balancing](load-balancer-authentication-access-control.md)
+ [Application Load Balancer metrics](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html#load-balancer-metrics-alb) in the *User Guide for Application Load Balancers*
+ [Network Load Balancer metrics](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-cloudwatch-metrics.html#load-balancer-metrics-nlb) in the *User Guide for Network Load Balancers*
+ [AWS::ElasticLoadBalancingV2::LoadBalancer](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-elasticloadbalancingv2-loadbalancer.html) in the *AWS CloudFormation User Guide*

## Step 4: Delete the Classic Load Balancer<a name="migrate-cleanup"></a>

You can delete the Classic Load Balancer after:
+ You have redirected all traffic to the new load balancer\.
+ All existing requests that were routed to the Classic Load Balancer have completed\.