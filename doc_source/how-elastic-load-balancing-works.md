# How Elastic Load Balancing Works<a name="how-elastic-load-balancing-works"></a>

A load balancer accepts incoming traffic from clients and routes requests to its registered targets \(such as EC2 instances\) in one or more Availability Zones\. The load balancer also monitors the health of its registered targets and ensures that it routes traffic only to healthy targets\. When the load balancer detects an unhealthy target, it stops routing traffic to that target, and then resumes routing traffic to that target when it detects that the target is healthy again\.

You configure your load balancer to accept incoming traffic by specifying one or more *listeners*\. A listener is a process that checks for connection requests\. It is configured with a protocol and port number for connections from clients to the load balancer and a protocol and port number for connections from the load balancer to the targets\.

Elastic Load Balancing supports three types of load balancers: Application Load Balancers, Network Load Balancers, and Classic Load Balancers\. There is a key difference between the way you configure these load balancers\. With Application Load Balancers and Network Load Balancers, you register targets in target groups, and route traffic to the target groups\. With Classic Load Balancers, you register instances with the load balancer\.

## Availability Zones and Load Balancer Nodes<a name="availability-zones"></a>

When you enable an Availability Zone for your load balancer, Elastic Load Balancing creates a load balancer node in the Availability Zone\. If you register targets in an Availability Zone but do not enable the Availability Zone, these registered targets do not receive traffic\. Note that your load balancer is most effective if you ensure that each enabled Availability Zone has at least one registered target\.

We recommend that you enable multiple Availability Zones\. \(Note that with an Application Load Balancer, we require you to enable multiple Availability Zones\.\) With this configuration, if one Availability Zone becomes unavailable or has no healthy targets, the load balancer can continue to route traffic to the healthy targets in another Availability Zone\.

After you disable an Availability Zone, the targets in that Availability Zone remain registered with the load balancer, but the load balancer will not route traffic to them\.

### Cross\-Zone Load Balancing<a name="cross-zone-load-balancing"></a>

If the nodes for your load balancer can distribute requests regardless of Availability Zone, this is known as *cross\-zone load balancing*\. With cross\-zone load balancing, the load balancer distributes traffic evenly across all registered targets in all enabled Availability Zones\. Otherwise, each load balancer node distributes traffic only to registered targets in its Availability Zone\. For example, suppose that you have 10 instances in us\-west\-2a and 2 instances in us\-west\-2b\. With cross\-zone load balancing, the load balancer distributes incoming requests evenly across all 12 instances\. Otherwise, the 2 instances in us\-west\-2b serve the same amount of traffic as the 10 instances in us\-west\-2a\.

With Application Load Balancers, cross\-zone load balancing is always enabled\.

With Network Load Balancers, each load balancer node distributes traffic across the registered targets in its Availability Zone only\.

When you create a Classic Load Balancer, the default for cross\-zone load balancing depends on how you create the load balancer\. With the API or CLI, cross\-zone load balancing is disabled by default\. With the AWS Management Console, the option to enable cross\-zone load balancing is selected by default\. After you create a Classic Load Balancer, you can enable or disable cross\-zone load balancing at any time\.

## Request Routing<a name="request-routing"></a>

Before a client sends a request to your load balancer, it resolves the load balancer's domain name using a Domain Name System \(DNS\) server\. The DNS entry is controlled by Amazon, because your load balancers are in the `amazonaws.com` domain\. The Amazon DNS servers return one or more IP addresses to the client, which are the IP addresses of the load balancer nodes for your load balancer\. With Network Load Balancers, Elastic Load Balancing creates a network interface for each Availability Zone you enable\. Each load balancer node in the Availability Zone uses this network interface to get a static IP address\. You can optionally associate one Elastic IP address with each network interface when you create the load balancer\.

As traffic to your application changes over time, Elastic Load Balancing scales your load balancer and updates the DNS entry\. Note that the DNS entry also specifies the time\-to\-live \(TTL\) as 60 seconds, which ensures that the IP addresses can be remapped quickly in response to changing traffic\.

The client determines which IP address to use to send requests to the load balancer\. The load balancer node that receives the request selects a healthy registered target and sends the request to the target using its private IP address\.

### Routing Algorithm<a name="routing-algorithm"></a>

With Application Load Balancers, the load balancer node that receives the request evaluates the listener rules in priority order to determine which rule to apply, and then selects a target from the target group for the rule action using the round robin routing algorithm\. Routing is performed independently for each target group, even when a target is registered with multiple target groups\.

With Network Load Balancers, the load balancer node that receives the connection selects a target from the target group for the default rule using a flow hash algorithm, based on the protocol, source IP address, source port, destination IP address, destination port, and TCP sequence number\. The TCP connections from a client have different source ports and sequence numbers, and can be routed to different targets\. Each individual TCP connection is routed to a single target for the life of the connection\.

With Classic Load Balancers, the load balancer node that receives the request selects a registered instance using the round robin routing algorithm for TCP listeners and the least outstanding requests routing algorithm for HTTP and HTTPS listeners\.

### HTTP Connections<a name="http-connections"></a>

Classic Load Balancers use pre\-open connections but Application Load Balancers do not\.

Classic Load Balancers support the following protocols on front\-end connections \(client to load balancer\): HTTP/0\.9, HTTP/1\.0, and HTTP/1\.1\.

Application Load Balancers support the following protocols on front\-end connections: HTTP/0\.9, HTTP/1\.0, HTTP/1\.1, and HTTP/2\. You can use HTTP/2 only with HTTPS listeners, and send up to 128 requests in parallel using one HTTP/2 connection\. Application Load Balancers also support connection upgrades from HTTP to Websockets\.

Both Application Load Balancers and Classic Load Balancers use HTTP/1\.1 on back\-end connections \(load balancer to registered target\)\. Keep\-alive is supported on back\-end connections by default\. For HTTP/1\.0 requests from clients that do not have a host header, the load balancer generates a host header for the HTTP/1\.1 requests sent on the back\-end connections\. For Application Load Balancer, the host header contains the DNS name of the load balancer\. For Classic Load Balancer, the host header contains the IP address of the load balancer node\.

You can set an idle timeout value for both Application Load Balancers and Classic Load Balancers\. The default value is 60 seconds\. With an Application Load Balancer, the idle timeout value applies only to front\-end connections\. With a Classic Load Balancer, if a front\-end connection or a back\-end connection is idle for longer than the idle timeout value, the connection is torn down and the client receives an error response\. A registered target can use a keep\-alive timeout to keep a back\-end connection open until it is ready to tear it down\.

Application Load Balancers and Classic Load Balancers support pipelined HTTP on front\-end connections\. They do not support pipelined HTTP on back\-end connections\.

### HTTP Headers<a name="http-headers"></a>

Application Load Balancers and Classic Load Balancers support **X\-Forwarded\-For**, **X\-Forwarded\-Proto**, and **X\-Forwarded\-Port** headers\.

For front\-end connections that use HTTP/2, the header names are in lowercase\. Before the request is sent to the target using HTTP/1\.1, the following header names are converted to mixed case: **X\-Forwarded\-For**, **X\-Forwarded\-Proto**, **X\-Forwarded\-Port**, **Host**, **X\-Amzn\-Trace\-Id**, **Upgrade**, and **Connection**\. All other header names are in lowercase\.

Application Load Balancers and Classic Load Balancers honor the connection header from the incoming client request after proxying the response back to the client\.

HTTP headers for Application Load Balancers have the following size limits:

+ Request line: 16K

+ Single header: 16K

+ Whole header: 64K

## Load Balancer Scheme<a name="load-balancer-scheme"></a>

When you create a load balancer, you must choose whether to make it an internal load balancer or an Internet\-facing load balancer\. Note that when you create a Classic Load Balancer in EC2\-Classic, it must be an Internet\-facing load balancer\.

The nodes of an Internet\-facing load balancer have public IP addresses\. The DNS name of an Internet\-facing load balancer is publicly resolvable to the public IP addresses of the nodes\. Therefore, Internet\-facing load balancers can route requests from clients over the Internet\.

The nodes of an internal load balancer have only private IP addresses\. The DNS name of an internal load balancer is publicly resolvable to the private IP addresses of the nodes\. Therefore, internal load balancers can only route requests from clients with access to the VPC for the load balancer\.

Note that both Internet\-facing and internal load balancers route requests to your targets using private IP addresses\. Therefore, your targets do not need public IP addresses to receive requests from an internal or an Internet\-facing load balancer\.

If your application has multiple tiers, for example web servers that must be connected to the Internet and database servers that are only connected to the web servers, you can design an architecture that uses both internal and Internet\-facing load balancers\. Create an Internet\-facing load balancer and register the web servers with it\. Create an internal load balancer and register the database servers with it\. The web servers receive requests from the Internet\-facing load balancer and send requests for the database servers to the internal load balancer\. The database servers receive requests from the internal load balancer\.