# Elastic Load Balancing API Permissions<a name="elb-api-permissions"></a>

You must grant IAM users permission to call the Elastic Load Balancing API actions they need, as described in [API Actions for Elastic Load Balancing](load-balancer-authentication-access-control.md#elb-api-actions)\. In addition, for some Elastic Load Balancing actions, you must grant IAM users permission to call specific actions from the Amazon EC2 API\.

## Required Permissions for the 2015\-12\-01 API<a name="required-permissions-v2"></a>

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

## Required Permissions for the 2012\-06\-01 API<a name="required-permissions-v1"></a>

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

## Example Policies<a name="example-policies"></a>

The following example policies show how you can control the permissions that IAM users have to use Elastic Load Balancing\.

**Example Example: Grant Full Access**  
The following policy grants users permission to use all Elastic Load Balancing API actions, the `CreateSecurityGroup` Amazon EC2 action, and all Amazon EC2 actions whose names begin with `Describe`\. This enables them to create, update, get information about, and delete Elastic Load Balancing resources\.  

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": [
          "elasticloadbalancing:*",
          "ec2:CreateSecurityGroup",
          "ec2:Describe*"
      ],
      "Resource": "*"
    }
   ]
}
```

**Example Example: Grant Read\-Only Access**  
The following policy grants users permission to use all Elastic Load Balancing and Amazon EC2 actions whose names begin with `Describe`\. This enables them to get information about Elastic Load Balancing resources\.  

```
{
   "Version": "2012-10-17",
   "Statement": [{
      "Effect": "Allow",
      "Action": [
          "elasticloadbalancing:Describe*",
          "ec2:Describe*"
      ],
      "Resource": "*"
    }
   ]
}
```