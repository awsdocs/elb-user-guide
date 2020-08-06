# Elastic Load Balancing and interface VPC endpoints<a name="load-balancer-vpc-endpoints"></a>

You can establish a private connection between your virtual private cloud \(VPC\) and the Elastic Load Balancing API by creating an interface VPC endpoint\. You can use this connection to call the Elastic Load Balancing API from your VPC without sending traffic over the internet\. The endpoint provides reliable, scalable connectivity to the Elastic Load Balancing API, versions 2015\-12\-01 and 2012\-06\-01\. It does this without requiring an internet gateway, NAT instance, or VPN connection\.

Interface VPC endpoints are powered by AWS PrivateLink, a feature that enables private communication between AWS services using private IP addresses\. For more information, see [AWS PrivateLink](http://aws.amazon.com/privatelink/)\.

**Limit**  
AWS PrivateLink does not support a Network Load Balancer with more than 50 listeners\.

## Create an interface endpoint for Elastic Load Balancing<a name="create-vpce-elb"></a>

Create an endpoint for Elastic Load Balancing using the following service name:

```
com.amazonaws.region.elasticloadbalancing
```

For more information, see [Creating an interface endpoint](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html#create-interface-endpoint) in the *Amazon VPC User Guide*\.

## Create a VPC endpoint policy for Elastic Load Balancing<a name="create-vpce-policy-elb"></a>

You can attach a policy to your VPC endpoint to control access to the Elastic Load Balancing API\. The policy specifies:
+ The principal that can perform actions\.
+ The actions that can be performed\.
+ The resource on which the actions can be performed\.

The following example shows a VPC endpoint policy that denies everyone permission to create a load balancer through the endpoint\. The example policy also grants everyone permission to perform all other actions\.

```
{
   "Statement": [
        {
            "Action": "*",
            "Effect": "Allow",
            "Resource": "*",
            "Principal": "*"
        },
        {
            "Action": "elasticloadbalancing:CreateLoadBalancer",
            "Effect": "Deny",
            "Resource": "*",
            "Principal": "*"
        }
    ]
}
```

For more information, see [Using VPC endpoint policies](https://docs.aws.amazon.com/vpc/latest/userguide/vpc-endpoints-access.html#vpc-endpoint-policies) in the *Amazon VPC User Guide*\.