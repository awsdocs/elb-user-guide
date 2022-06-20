# AWS managed policies for Elastic Load Balancing<a name="managed-policies"></a>

To add permissions to users, groups, and roles, it is easier to use AWS managed policies than to write policies yourself\. It takes time and expertise to [create IAM customer managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_create-console.html) that provide your team with only the permissions they need\. To get started quickly, you can use our AWS managed policies\. These policies cover common use cases and are available in your AWS account\. For more information about AWS managed policies, see [AWS managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html#aws-managed-policies) in the *IAM User Guide*\.

AWS services maintain and update AWS managed policies\. You can't change the permissions in AWS managed policies\. Services can add additional permissions to an AWS managed policy to support new features\. This type of update affects all identities \(users, groups, and roles\) where the policy is attached\. Services are most likely to update an AWS managed policy when a new feature is launched or when new operations become available\. Services do not remove permissions from an AWS managed policy, so policy updates won't break your existing permissions\.

Additionally, AWS supports managed policies for job functions that span multiple services\. For example, the **ReadOnlyAccess** AWS managed policy provides read\-only access to all AWS services and resources\. When a service launches a new feature, AWS adds read\-only permissions for new operations and resources\. For a list of job function policies and their descriptions, see [AWS managed policies for job functions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_job-functions.html) in the *IAM User Guide*\.

## AWS managed policy: AWSElasticLoadBalancingClassicServiceRolePolicy<a name="AWSElasticLoadBalancingClassicServiceRolePolicy"></a>

This policy includes all the permissions that Elastic Load Balancing \(Classic Load Balancer\) requires to call other AWS services on your behalf\. Service\-linked roles are predefined\. With predefined roles you don't have to manually add the necessary permissions for Elastic Load Balancing to complete actions on your behalf\. You cannot attach, detach, modify, or delete this policy\. 

To view the permissions for this policy, see [AWSElasticLoadBalancingClassicServiceRolePolicy](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSElasticLoadBalancingClassicServiceRolePolicy) in the AWS Management Console\.

## AWS managed policy: AWSElasticLoadBalancingServiceRolePolicy<a name="AWSElasticLoadBalancingServiceRolePolicy"></a>

This policy includes all the permissions that Elastic Load Balancing requires to call other AWS services on your behalf\. Service\-linked roles are predefined\. With predefined roles you don't have to manually add the necessary permissions for Elastic Load Balancing to complete actions on your behalf\. You cannot attach, detach, modify, or delete this policy\. 

To view the permissions for this policy, see [AWSElasticLoadBalancingServiceRolePolicy](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/AWSElasticLoadBalancingServiceRolePolicy) in the AWS Management Console\.

## AWS managed policy: ElasticLoadBalancingFullAccess<a name="ElasticLoadBalancingFullAccess"></a>

This policy gives full access to the Elastic Load Balancing service and limited access to other services via the AWS Management Console\.

To view the permissions for this policy, see [ElasticLoadBalancingFullAccess](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/ElasticLoadBalancingFullAccess) in the AWS Management Console\.

## AWS managed policy: ElasticLoadBalancingReadOnly<a name="ElasticLoadBalancingReadOnly"></a>

This policy provides read\-only access to Elastic Load Balancing and dependent services\.

To view the permissions for this policy, see [ElasticLoadBalancingReadOnly](https://console.aws.amazon.com/iam/home#/policies/arn:aws:iam::aws:policy/ElasticLoadBalancingReadOnly) in the AWS Management Console\.

## Elastic Load Balancing updates to AWS managed policies<a name="policy-updates"></a>

View details about updates to AWS managed policies for Elastic Load Balancing since this service began tracking these changes\.


| Change | Description | Date | 
| --- | --- | --- | 
|  [AWS managed policy: AWSElasticLoadBalancingServiceRolePolicy](#AWSElasticLoadBalancingServiceRolePolicy) \- Update to an existing policy\.  |  Elastic Load Balancing added a new action to grant permissions to use peering connections\. This action was added to the service\-linked role policy, for Elastic Load Balancing control plane\. It is associated with the `ec2:DescribeVpcPeeringConnections` API operation\.   | October 11, 2021 | 
|  [AWS managed policy: ElasticLoadBalancingFullAccess](#ElasticLoadBalancingFullAccess) \- Update to an existing policy\.  |  Elastic Load Balancing added a new action to grant permissions to use peering connections\. This action was added to the Elastic Load Balancing full access policy\. It is associated with the `ec2:DescribeVpcPeeringConnections` API operation\.   | October 11, 2021 | 
|  [AWS managed policy: AWSElasticLoadBalancingClassicServiceRolePolicy](#AWSElasticLoadBalancingClassicServiceRolePolicy) \- Update to an existing policy\.  |  Elastic Load Balancing added a service\-linked role policy \(for the control plane\) for the Classic Load Balancer\. This update is for version 2 \(default\)\.   |  October 7, 2019  | 
|  [AWS managed policy: ElasticLoadBalancingReadOnly](#ElasticLoadBalancingReadOnly)  |  Provides read\-only access to Elastic Load Balancing and dependent services\. This is the version 1 \(default\)\.   |  September 20, 2018   | 
|  Elastic Load Balancing started tracking changes  |  Elastic Load Balancing started tracking changes for its AWS managed policies\.   |  July 23, 2021   | 