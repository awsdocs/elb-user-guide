# Identity and access management for Elastic Load Balancing<a name="load-balancer-authentication-access-control"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way\. You can do this without sharing your security credentials\.

By default, IAM users don't have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources such as a load balancer, and to perform tasks, you:

1. Create an IAM policy that grants the IAM user permission to use the specific resources and API actions they need\.

1. Attach the policy to the IAM user or the group that the IAM user belongs to\.

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

For example, you can use IAM to create users and groups under your AWS account\. An IAM user can be a person, a system, or an application\. Then you grant permissions to the users and groups to perform specific actions on the specified resources using an IAM policy\.

## Grant permissions using IAM policies<a name="elb-iam-policies"></a>

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as shown in the following example\.

```
{
  "Version": "2012-10-17",
  "Statement":[{
    "Effect": "effect",
    "Action": "action",
    "Resource": "resource-arn",
    "Condition": {
      "condition": {
        "key":"value"
      }
    }
  }]
}
```
+ **Effect**— The *effect* can be `Allow` or `Deny`\. By default, IAM users don't have permission to use resources and API actions, so all requests are denied\. An explicit allow overrides the default\. An explicit deny overrides any allows\.
+ **Action**— The *action* is the specific API action for which you are granting or denying permission\. For more information about specifying *action*, see [API actions for Elastic Load Balancing](#elb-api-actions)\.
+ **Resource**— The resource that's affected by the action\. With many Elastic Load Balancing API actions, you can restrict the permissions granted or denied to a specific load balancer\. To do so, specify its Amazon Resource Name \(ARN\) in this statement\. Otherwise, you can use the \* wildcard to specify all of your load balancers\. For more information, see [Elastic Load Balancing resources](#elb-resources)\.
+ **Condition**— You can optionally use conditions to control when your policy is in effect\. For more information, see [Condition keys for Elastic Load Balancing](#elb-condition-keys)\.

For more information, see the [IAM User Guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/)\.

## API actions for Elastic Load Balancing<a name="elb-api-actions"></a>

In the **Action** element of your IAM policy statement, you can specify any API action that Elastic Load Balancing offers\. You must prefix the action name with the lowercase string `elasticloadbalancing:`, as shown in the following example\.

```
"Action": "elasticloadbalancing:DescribeLoadBalancers"
```

To specify multiple actions in a single statement, enclose them in square brackets and separate them with a comma, as shown in the following example\.

```
"Action": [
    "elasticloadbalancing:DescribeLoadBalancers",
    "elasticloadbalancing:DeleteLoadBalancer"
]
```

You can also specify multiple actions using the \* wildcard\. The following example specifies all API action names for Elastic Load Balancing that start with `Describe`\.

```
"Action": "elasticloadbalancing:Describe*"
```

To specify all API actions for Elastic Load Balancing, use the \* wildcard, as shown in the following example\.

```
"Action": "elasticloadbalancing:*"
```

For the complete list of the API actions for Elastic Load Balancing, see the following documentation:
+ Application Load Balancers and Network Load Balancers — [API Reference version 2015\-12\-01](https://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)
+ Classic Load Balancers — [API Reference version 2012\-06\-01](https://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/)

## Elastic Load Balancing resources<a name="elb-resources"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Elastic Load Balancing has partial support for resource\-level permissions\. For API actions that support resource\-level permissions, you can control the resources that users are allowed to use with the action\. To specify a resource in a policy statement, you must use its Amazon Resource Name \(ARN\)\. When specifying an ARN, you can use the \* wildcard in your paths\. For example, you can use the \* wildcard when you do not want to specify the exact load balancer name\.

The ARN for an Application Load Balancer has the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:loadbalancer/app/load-balancer-name/load-balancer-id
```

The ARN for a Network Load Balancer has the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:loadbalancer/net/load-balancer-name/load-balancer-id
```

The ARN for a Classic Load Balancer has the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:loadbalancer/load-balancer-name
```

The ARNs for a listener and a listener rule for an Application Load Balancer have the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:listener/app/load-balancer-name/load-balancer-id/listener-id
arn:aws:elasticloadbalancing:region-code:account-id:listener-rule/app/load-balancer-name/load-balancer-id/listener-id/rule-id
```

The ARN for a listener for a Network Load Balancer has the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:listener/net/load-balancer-name/load-balancer-id/listener-id
```

The ARN for a target group has the format shown in the following example\.

```
arn:aws:elasticloadbalancing:region-code:account-id:targetgroup/target-group-name/target-group-id
```

**API actions with no support for resource\-level permissions**

The following Elastic Load Balancing actions do not support resource\-level permissions:
+ API version 2015\-12\-01:
  + `DescribeAccountLimits`
  + `DescribeListenerCertificates`
  + `DescribeListeners`
  + `DescribeLoadBalancerAttributes`
  + `DescribeLoadBalancers`
  + `DescribeRules`
  + `DescribeSSLPolicies`
  + `DescribeTags`
  + `DescribeTargetGroupAttributes`
  + `DescribeTargetGroups`
  + `DescribeTargetHealth`
+ API version 2012\-06\-01:
  + `DescribeInstanceHealth`
  + `DescribeLoadBalancerAttributes`
  + `DescribeLoadBalancerPolicyTypes`
  + `DescribeLoadBalancers`
  + `DescribeLoadBalancerPolicies`
  + `DescribeTags`

For API actions that don't support resource\-level permissions, you must specify the resource statement shown in the following example\.

```
"Resource": "*"
```

## Resource\-level permissions for Elastic Load Balancing<a name="elb-resource-level-permissions"></a>

The following tables describe the Elastic Load Balancing actions that support resource\-level permissions, and the supported resources for each action\.


**API version 2015\-12\-01**  

| API action | Resource ARNs | 
| --- | --- | 
| AddListenerCertificates | listener | 
| AddTags | load balancer, target group | 
| CreateListener | load balancer | 
| CreateLoadBalancer | load balancer | 
| CreateRule | listener | 
| CreateTargetGroup | target group | 
| DeleteListener | listener | 
| DeleteLoadBalancer | load balancer | 
| DeleteRule | listener rule | 
| DeleteTargetGroup | target group | 
| DeregisterTargets | target group | 
| ModifyListener | listener | 
| ModifyLoadBalancerAttributes | load balancer | 
| ModifyRule | listener rule | 
| ModifyTargetGroup | target group | 
| ModifyTargetGroupAttributes | target group | 
| RegisterTargets | target group | 
| RemoveListenerCertificates | listener | 
| RemoveTags | load balancer, target group | 
| SetIpAddressType | load balancer | 
| SetRulePriorities | listener rule | 
| SetSecurityGroups | load balancer | 
| SetSubnets | load balancer | 


**API version 2012\-06\-01**  

| API action | Resource ARNs | 
| --- | --- | 
| AddTags | load balancer | 
| ApplySecurityGroupsToLoadBalancer | load balancer | 
| AttachLoadBalancerToSubnets | load balancer | 
| ConfigureHealthCheck | load balancer | 
| CreateAppCookieStickinessPolicy | load balancer | 
| CreateLBCookieStickinessPolicy | load balancer | 
| CreateLoadBalancer | load balancer | 
| CreateLoadBalancerListeners | load balancer | 
| CreateLoadBalancerPolicy | load balancer | 
| DeleteLoadBalancer | load balancer | 
| DeleteLoadBalancerListeners | load balancer | 
| DeleteLoadBalancerPolicy | load balancer | 
| DeregisterInstancesFromLoadBalancer | load balancer | 
| DetachLoadBalancerFromSubnets | load balancer | 
| DisableAvailabilityZonesForLoadBalancer | load balancer | 
| EnableAvailabilityZonesForLoadBalancer | load balancer | 
| ModifyLoadBalancerAttributes | load balancer | 
| RegisterInstancesWithLoadBalancer | load balancer | 
| RemoveTags | load balancer | 
| SetLoadBalancerListenerSSLCertificate | load balancer | 
| SetLoadBalancerPoliciesForBackendServer | load balancer | 
| SetLoadBalancerPoliciesOfListener | load balancer | 

## Condition keys for Elastic Load Balancing<a name="elb-condition-keys"></a>

When you create a policy, you can specify the conditions that control when the policy is in effect\. Each condition contains one or more key\-value pairs\. There are global condition keys and service\-specific condition keys\.

You cannot use the `aws:SourceIp` condition key with Elastic Load Balancing\.

The `elasticloadbalancing:ResourceTag`/*key* condition key is specific to Elastic Load Balancing\. The following actions support this condition key:

**API version 2015\-12\-01**
+ `AddTags`
+ `CreateListener`
+ `CreateLoadBalancer`
+ `DeleteLoadBalancer`
+ `DeleteTargetGroup`
+ `DeregisterTargets`
+ `ModifyLoadBalancerAttributes`
+ `ModifyTargetGroup`
+ `ModifyTargetGroupAttributes`
+ `RegisterTargets`
+ `RemoveTags`
+ `SetIpAddressType`
+ `SetSecurityGroups`
+ `SetSubnets`

**API version 2012\-06\-01**
+ `AddTags`
+ `ApplySecurityGroupsToLoadBalancer`
+ `AttachLoadBalancersToSubnets`
+ `ConfigureHealthCheck`
+ `CreateAppCookieStickinessPolicy`
+ `CreateLBCookieStickinessPolicy`
+ `CreateLoadBalancer`
+ `CreateLoadBalancerListeners`
+ `CreateLoadBalancerPolicy`
+ `DeleteLoadBalancer`
+ `DeleteLoadBalancerListeners`
+ `DeleteLoadBalancerPolicy`
+ `DeregisterInstancesFromLoadBalancer`
+ `DetachLoadBalancersFromSubnets`
+ `DisableAvailabilityZonesForLoadBalancer`
+ `EnableAvailabilityZonesForLoadBalancer`
+ `ModifyLoadBalancerAttributes`
+ `RegisterInstancesWithLoadBalancer`
+ `RemoveTags`
+ `SetLoadBalancerListenerSSLCertificate`
+ `SetLoadBalancerPoliciesForBackendServer`
+ `SetLoadBalancerPoliciesOfListener`

For more information about global condition keys, see [AWS global condition context keys](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html) in the *IAM User Guide*\.

The following actions support the `aws:RequestTag`/*key* and `aws:TagKeys` condition keys:
+ `AddTags`
+ `CreateLoadBalancer`
+ `RemoveTags`

## Predefined AWS managed policies<a name="elb-predefined-policies"></a>

The managed policies created by AWS grant the required permissions for common use cases\. You can attach these policies to your IAM users, based on the access to Elastic Load Balancing that they require:
+ **ElasticLoadBalancingFullAccess** — Grants full access required to use Elastic Load Balancing features\.
+ **ElasticLoadBalancingReadOnly** — Grants read\-only access to Elastic Load Balancing features\.

For more information about the permissions required by each Elastic Load Balancing action, see [Elastic Load Balancing API permissions](elb-api-permissions.md)\.