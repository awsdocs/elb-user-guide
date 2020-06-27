# Elastic Load Balancing API permissions<a name="elb-api-permissions"></a>

You must grant IAM users permission to call the Elastic Load Balancing API actions they need, as described in [API actions for Elastic Load Balancing](load-balancer-authentication-access-control.md#elb-api-actions)\. In addition, for some Elastic Load Balancing actions, you must grant IAM users permission to call specific actions from the Amazon EC2 API\.

## Required permissions for the 2015\-12\-01 API<a name="required-permissions-v2"></a>

When calling the following actions from the 2015\-12\-01 API, you must grant IAM users permission to call the specified actions\.

`CreateLoadBalancer`  
+ `elasticloadbalancing:CreateLoadBalancer`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeAddresses`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeVpcs`
+ `iam:CreateServiceLinkedRole`

`CreateTargetGroup`  
+ `elasticloadbalancing:CreateTargetGroup`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeVpcs`

`RegisterTargets`  
+ `elasticloadbalancing:RegisterTargets`
+ `ec2:DescribeInstances`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeVpcs`

`SetIpAddressType`  
+ `elasticloadbalancing:SetIpAddressType`
+ `ec2:DescribeSubnets`

`SetSubnets`  
+ `elasticloadbalancing:SetSubnets`
+ `ec2:DescribeSubnets`

## Required permissions for the 2012\-06\-01 API<a name="required-permissions-v1"></a>

When calling the following actions from the 2012\-06\-01 API, you must grant IAM users permission to call the specified actions\.

`ApplySecurityGroupsToLoadBalancer`  
+ `elasticloadbalancing:ApplySecurityGroupsToLoadBalancer`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeSecurityGroups`

`AttachLoadBalancerToSubnets`  
+ `elasticloadbalancing:AttachLoadBalancerToSubnets`
+ `ec2:DescribeSubnets`

`CreateLoadBalancer`  
+ `elasticloadbalancing:CreateLoadBalancer`
+ `ec2:CreateSecurityGroup`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeVpcs`
+ `iam:CreateServiceLinkedRole`

`DeregisterInstancesFromLoadBalancer`  
+ `elasticloadbalancing:DeregisterInstancesFromLoadBalancer`
+ `ec2:DescribeClassicLinkInstances`
+ `ec2:DescribeInstances`

`DescribeInstanceHealth`  
+ `elasticloadbalancing:DescribeInstanceHealth`
+ `ec2:DescribeClassicLinkInstances`
+ `ec2:DescribeInstances`

`DescribeLoadBalancers`  
+ `elasticloadbalancing:DescribeLoadBalancers`
+ `ec2:DescribeSecurityGroups`

`DisableAvailabilityZonesForLoadBalancer`  
+ `elasticloadbalancing:DisableAvailabilityZonesForLoadBalancer`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeVpcs`

`EnableAvailabilityZonesForLoadBalancer`  
+ `elasticloadbalancing:EnableAvailabilityZonesForLoadBalancer`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeVpcs`

`RegisterInstancesWithLoadBalancer`  
+ `elasticloadbalancing:RegisterInstancesWithLoadBalancer`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeClassicLinkInstances`
+ `ec2:DescribeInstances`
+ `ec2:DescribeVpcClassicLink`