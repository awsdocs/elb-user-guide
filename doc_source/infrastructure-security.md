# Infrastructure security in Elastic Load Balancing<a name="infrastructure-security"></a>

As a managed service, Elastic Load Balancing is protected by the AWS global network security procedures that are described in the [Amazon Web Services: Overview of security processes](https://d0.awsstatic.com/whitepapers/Security/AWS_Security_Whitepaper.pdf) whitepaper\.

You use AWS published API calls to access Elastic Load Balancing through the network\. Clients must support Transport Layer Security \(TLS\) 1\.0 or later\. We recommend TLS 1\.2 or later\. Clients must also support cipher suites with perfect forward secrecy \(PFS\) such as Ephemeral Diffie\-Hellman \(DHE\) or Elliptic Curve Ephemeral Diffie\-Hellman \(ECDHE\)\. Most modern systems such as Java 7 and later support these modes\.

Additionally, requests must be signed using an access key ID and a secret access key that is associated with an IAM principal\. Or you can use the [AWS Security Token Service](https://docs.aws.amazon.com/STS/latest/APIReference/Welcome.html) \(AWS STS\) to generate temporary security credentials to sign requests\.

## Network isolation<a name="network-isolation"></a>

A virtual private cloud \(VPC\) is a virtual network in your own logically isolated area in the AWS Cloud\. A subnet is a range of IP addresses in a VPC\. When you create a load balancer, you can specify one or more subnets for the load balancer nodes\. You can deploy EC2 instances in the subnets of your VPC and register them with your load balancer\. For more information about VPC and subnets, see the [Amazon VPC User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/)\.

When you create a load balancer in a VPC, it can be either internet\-facing or internal\. An internal load balancer can only route requests that come from clients with access to the VPC for the load balancer\.

Your load balancer sends requests to its registered targets using private IP addresses\. Therefore, your targets do not need public IP addresses in order to receive requests from a load balancer\.

To call the Elastic Load Balancing API from your VPC without sending traffic over the public internet, use AWS PrivateLink\. For more information, see [Elastic Load Balancing and interface VPC endpoints](load-balancer-vpc-endpoints.md)\.

## Controlling network traffic<a name="control-network-traffic"></a>

Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. Application Load Balancers operate at the request level \(layer 7\) of the Open Systems Interconnection \(OSI\) model\. Network Load Balancers operate at the connection level \(layer 4\) of the OSI model\. Classic Load Balancers operate at both the request and connection levels\.

Consider the following options for securing network traffic when you use a load balancer:
+ Use secure listeners to support encrypted communication between clients and your load balancers\. Application Load Balancers support HTTPS listeners\. Network Load Balancers support TLS listeners\. Classic Load Balancers support both HTTPS and TLS listeners\. You can choose from predefined security policies for your load balancer to specify the cipher suites and protocol versions that are supported by your application\. You can use AWS Certificate Manager \(ACM\) or AWS Identity and Access Management \(IAM\) to manage the server certificates installed on your load balancer\. You can use the Server Name Indication \(SNI\) protocol to serve multiple secure websites using a single secure listener\. SNI is automatically enabled for your load balancer when you associate more than one server certificate with a secure listener\.
+ Configure the security groups for your Application Load Balancers and Classic Load Balancers to accept traffic only from specific clients\. These security groups must allow inbound traffic from clients on the listener ports and outbound traffic to the clients\.
+ Configure the security groups for your Amazon EC2 instances to accept traffic only from the load balancer\. These security groups must allow inbound traffic from the load balancer on the listener ports and the health check ports\.
+ Configure your Application Load Balancer to securely authenticate users through an identity provider or using corporate identities\. For more information, see [Authenticate users using an Application Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/listener-authenticate-users.html)\.
+ Use [AWS WAF](https://docs.aws.amazon.com/waf/latest/developerguide/waf-chapter.html) with your Application Load Balancers to allow or block requests based on the rules in a web access control list \(web ACL\)\.