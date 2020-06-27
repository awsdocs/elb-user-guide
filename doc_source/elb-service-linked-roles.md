# Elastic Load Balancing service\-linked role<a name="elb-service-linked-roles"></a>

Elastic Load Balancing uses a service\-linked role for the permissions that it requires to call other AWS services on your behalf\. For more information, see [Using service\-linked roles](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html) in the *IAM User Guide*\.

## Permissions granted by the service\-linked role<a name="service-linked-role-permissions"></a>

Elastic Load Balancing uses the service\-linked role named **AWSServiceRoleForElasticLoadBalancing** to call the following actions on your behalf:
+ `ec2:DescribeAddresses`
+ `ec2:DescribeInstances`
+ `ec2:DescribeNetworkInterfaces`
+ `ec2:DescribeSubnets`
+ `ec2:DescribeSecurityGroups`
+ `ec2:DescribeVpcs`
+ `ec2:DescribeInternetGateways`
+ `ec2:DescribeAccountAttributes`
+ `ec2:DescribeClassicLinkInstances`
+ `ec2:DescribeVpcClassicLink`
+ `ec2:CreateSecurityGroup`
+ `ec2:CreateNetworkInterface`
+ `ec2:DeleteNetworkInterface`
+ `ec2:ModifyNetworkInterfaceAttribute`
+ `ec2:AuthorizeSecurityGroupIngress`
+ `ec2:AssociateAddress`
+ `ec2:DisassociateAddress`
+ `ec2:AttachNetworkInterface`
+ `ec2:DetachNetworkInterface`
+ `ec2:AssignPrivateIpAddresses`
+ `ec2:AssignIpv6Addresses`
+ `ec2:UnassignIpv6Addresses`
+ `logs:CreateLogDelivery`
+ `logs:GetLogDelivery`
+ `logs:UpdateLogDelivery`
+ `logs:DeleteLogDelivery`
+ `logs:ListLogDeliveries`

**AWSServiceRoleForElasticLoadBalancing** trusts the `elasticloadbalancing.amazonaws.com` service to assume the role\.

## Create the service\-linked role<a name="create-service-linked-role"></a>

You don't need to manually create the **AWSServiceRoleForElasticLoadBalancing** role\. Elastic Load Balancing creates this role for you when you create a load balancer\.

For Elastic Load Balancing to create a service\-linked role on your behalf, you must have the required permissions\. For more information, see [Service\-linked role permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#service-linked-role-permissions) in the *IAM User Guide*\.

If you created a load balancer before January 11, 2018, Elastic Load Balancing created **AWSServiceRoleForElasticLoadBalancing** in your AWS account\. For more information, see [A new role appeared in my AWS account](https://docs.aws.amazon.com/IAM/latest/UserGuide/troubleshoot_roles.html#troubleshoot_roles_new-role-appeared) in the *IAM User Guide*\.

## Edit the service\-linked role<a name="edit-service-linked-role"></a>

You can edit the description of **AWSServiceRoleForElasticLoadBalancing** using IAM\. For more information, see [Editing a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#edit-service-linked-role) in the *IAM User Guide*\.

## Delete the service\-linked role<a name="delete-service-linked-role"></a>

If you no longer need to use Elastic Load Balancing, we recommend that you delete **AWSServiceRoleForElasticLoadBalancing**\.

You can delete this service\-linked role only after you delete all load balancers in your AWS account\. This ensures that you can't inadvertently remove permission to access your load balancers\. For more information, see [Delete an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-delete.html), [Delete a Network Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-delete.html), and [Delete a Classic Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-getting-started.html#delete-load-balancer)\.

You can use the IAM console, the IAM CLI, or the IAM API to delete service\-linked roles\. For more information, see [Deleting a service\-linked role](https://docs.aws.amazon.com/IAM/latest/UserGuide/using-service-linked-roles.html#delete-service-linked-role) in the *IAM User Guide*\.

After you delete **AWSServiceRoleForElasticLoadBalancing**, Elastic Load Balancing creates the role again if you create a load balancer\.