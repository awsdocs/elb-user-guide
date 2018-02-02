# Authentication and Access Control for Your Load Balancers<a name="load-balancer-authentication-access-control"></a>

AWS uses security credentials to identify you and to grant you access to your AWS resources\. You can use features of AWS Identity and Access Management \(IAM\) to allow other users, services, and applications to use your AWS resources fully or in a limited way, without sharing your security credentials\.

By default, IAM users don't have permission to create, view, or modify AWS resources\. To allow an IAM user to access resources, such as a load balancer, and perform tasks, you must create an IAM policy that grants the IAM user permission to use the specific resources and API actions they'll need, then attach the policy to the IAM user or the group the IAM user belongs to\. When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

For example, you can use IAM to create users and groups under your AWS account \(an IAM user can be a person, a system, or an application\)\. Then you grant permissions to the users and groups to perform specific actions on the specified resources using an IAM policy\.

## Grant Permissions Using IAM Policies<a name="elb-iam-policies"></a>

When you attach a policy to a user or group of users, it allows or denies the users permission to perform the specified tasks on the specified resources\.

An IAM policy is a JSON document that consists of one or more statements\. Each statement is structured as follows:

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

+ **Action**— The *action* is the specific API action for which you are granting or denying permission\. For more information about specifying *action*, see [Actions for Elastic Load Balancing](#elb-api-actions)\.

+ **Resource**— The resource that's affected by the action\. With many Elastic Load Balancing API actions, you can restrict the permissions granted or denied to a specific load balancer by specifying its Amazon Resource Name \(ARN\) in this statement\. Otherwise, you can use the \* wildcard to specify all of your load balancers\. For more information, see [Resource\-Level Permissions for Elastic Load Balancing](#elb-resource-level-permissions)\.

+ **Condition**— You can optionally use conditions to control when your policy is in effect\. For more information, see [Condition Keys for Elastic Load Balancing](#elb-condition-keys)\.

For more information, see [What is IAM?](http://docs.aws.amazon.com/IAM/latest/UserGuide/IAM_Introduction.html)\. For information about creating and managing users and groups, see [IAM Users and Groups](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_WorkingWithGroupsAndUsers.html)\.

## Actions for Elastic Load Balancing<a name="elb-api-actions"></a>

In the **Action** element of your IAM policy statement, you can specify any API action that Elastic Load Balancing offers\. You must prefix the action name with the lowercase string `elasticloadbalancing:`, as shown in the following example:

```
"Action": "elasticloadbalancing:DescribeLoadBalancers"
```

To specify multiple actions in a single statement, enclose them in square brackets and separate them with a comma, as follows:

```
"Action": ["elasticloadbalancing:action1","elasticloadbalancing:action2"]
```

You can also specify multiple actions using the \* wildcard\. The following example specifies all API action names for Elastic Load Balancing that start with `Describe`:

```
"Action": "elasticloadbalancing:Describe*"
```

To specify all API actions for Elastic Load Balancing, use the \* wildcard, as in the following example:

```
"Action": "elasticloadbalancing:*"
```

For the complete list of the API actions for Elastic Load Balancing, see the following documentation:

+ Application Load Balancers and Network Load Balancers — [API Reference version 2015\-12\-01](http://docs.aws.amazon.com/elasticloadbalancing/latest/APIReference/)

+ Classic Load Balancers — [API Reference version 2012\-06\-01](http://docs.aws.amazon.com/elasticloadbalancing/2012-06-01/APIReference/)

## Resource\-Level Permissions for Elastic Load Balancing<a name="elb-resource-level-permissions"></a>

*Resource\-level permissions* refers to the ability to specify which resources users are allowed to perform actions on\. Elastic Load Balancing has partial support for resource\-level permissions\.

**Important**  
There are two API sets for Elastic Load Balancing: one for Application Load Balancers and Network Load Balancers \(API version 2015\-12\-01\), and one for Classic Load Balancers \(API version 2012\-06\-01\)\. Currently, the 2015\-12\-01 API does not support resource\-level permissions\. The version 2012\-06\-01 API supports resource\-level permissions; however, not every API action supports resource\-level permissions\.

For API actions that support resource\-level permissions, you can control the load balancers that users are allowed to use with the action\. To specify a load balancer in a policy statement, you must use its Amazon Resource Name \(ARN\)\. When specifying an ARN, you can use the \* wildcard in your paths; for example, when you do not want to specify the exact load balancer name\.

**ARN Syntax**  
The ARN for a Classic Load Balancer has the following syntax:

```
arn:aws:elasticloadbalancing:region:my-account-id:loadbalancer/load-balancer-name
```

*region*  
The region for the load balancer\. For more information about the regions supported by Elastic Load Balancing, see [Regions and Endpoints](http://docs.aws.amazon.com/general/latest/gr/rande.html#elb_region) in the *Amazon Web Services General Reference*\.

*account\-id*  
Your AWS account ID, with no hyphens \(for example, `0123456789012`\)\.

*load\-balancer\-name*  
The name of your load balancer\.

**ARN Example**  
The following is an example ARN for a Classic Load Balancer:

```
"Resource": "arn:aws:elasticloadbalancing:us-east-2:123456789012:loadbalancer/my-load-balancer"
```

**API Actions with No Support for Resource\-Level Permissions**  
You can't specify the ARN for a specific load balancer using the following API actions:

+ All API actions for API version 2015\-12\-01

+ The following API actions for API version 2012\-06\-01:

  + `DescribeInstanceHealth`

  + `DescribeLoadBalancerAttributes`

  + `DescribeLoadBalancerPolicyTypes`

  + `DescribeLoadBalancers`

  + `DescribeLoadBalancerPolicies`

  + `DescribeTags`

For API actions that don't support resource\-level permissions, you must specify the following resource statement:

```
"Resource": "*"
```

## Condition Keys for Elastic Load Balancing<a name="elb-condition-keys"></a>

In an IAM policy statement, you have the option to specify conditions that control when it is in effect\. Each condition contains one or more key\-value pairs\. AWS has defined the following keys that you can use to specify the conditions under which your IAM users and groups can use the load balancers\.

**Tip**  
Key names are case\-sensitive\.

+ `aws:CurrentTime`— Use with date/time conditions to restrict access based on request time \(see [Date Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_Date)\)\.

+ `aws:EpochTime`— Use with date/time conditions to specify a date in epoch or UNIX time \(see [Date Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_Date)\)\. 

+ `aws:MultiFactorAuthPresent`— Use to check whether the IAM user making the API request was authenticated using a multi\-factor authentication \(MFA\) device\. 

+ `aws:MultiFactorAuthAge`— Use to check how long ago \(in seconds\) the MFA\-validated security credentials making the request were issued\. Unlike other keys, this key is not present if MFA is not used \(see [Existence of Condition Keys](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_Null), [Numeric Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_Numeric), and [Using Multi\-Factor Authentication \(MFA\) Devices with AWS](http://docs.aws.amazon.com/IAM/latest/UserGuide/Using_ManagingMFA.html)\)\.

+ `aws:SecureTransport`— Use to check whether the request was sent using SSL \(see [Boolean Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_Boolean)\)\.

+ `aws:SourceIp`— Use to check the requester's IP address \(see [IP Address](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_IPAddress)\)\. Note that if you use `aws:SourceIp`, and the request comes from an EC2 instance, the public IP address of the instance is evaluated\.

+ `aws:Referer`— Use to check the user making the HTTP request\.

+ `aws:UserAgent`— Use with string conditions to check the client application that made the request \(see [String Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_String)\)\.

+ `aws:userid`— Use to check the user ID of the requester \(see [String Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_String)\)\.

+ `aws:username`—Use to check the user name of the requester, if available \(see [String Conditions](http://docs.aws.amazon.com/IAM/latest/UserGuide/AccessPolicyLanguage_ElementDescriptions.html#Conditions_String)\)\.

After you have decided how to control access to your load balancers, open the [IAM console](https://console.aws.amazon.com/iam) and follow the directions in [Managing IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/ManagingPolicies.html) to create an IAM policy for Elastic Load Balancing\. For more information, see [Testing IAM Policies](http://docs.aws.amazon.com/IAM/latest/UserGuide/testing-policies.html)\.