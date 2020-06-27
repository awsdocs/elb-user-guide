# How Elastic Load Balancing works<a name="how-elastic-load-balancing-works"></a>

A load balancer accepts incoming traffic from clients and routes requests to its registered targets \(such as EC2 instances\) in one or more Availability Zones\. The load balancer also monitors the health of its registered targets and ensures that it routes traffic only to healthy targets\. When the load balancer detects an unhealthy target, it stops routing traffic to that target\. It then resumes routing traffic to that target when it detects that the target is healthy again\.

You configure your load balancer to accept incoming traffic by specifying one or more *listeners*\. A listener is a process that checks for connection requests\. It is configured with a protocol and port number for connections from clients to the load balancer\. Likewise, it is configured with a protocol and port number for connections from the load balancer to the targets\.

Elastic Load Balancing supports three types of load balancers:
+ Application Load Balancers
+ Network Load Balancers
+ Classic Load Balancers

There is a key difference in how the load balancer types are configured\. With Application Load Balancers and Network Load Balancers, you register targets in target groups, and route traffic to the target groups\. With Classic Load Balancers, you register instances with the load balancer\.

## Availability Zones and load balancer nodes<a name="availability-zones"></a>

When you enable an Availability Zone for your load balancer, Elastic Load Balancing creates a load balancer node in the Availability Zone\. If you register targets in an Availability Zone but do not enable the Availability Zone, these registered targets do not receive traffic\. Your load balancer is most effective when you ensure that each enabled Availability Zone has at least one registered target\.

We recommend that you enable multiple Availability Zones\. \(With an Application Load Balancer, we require you to enable multiple Availability Zones\.\) This configuration helps ensure that the load balancer can continue to route traffic\. If one Availability Zone becomes unavailable or has no healthy targets, the load balancer can route traffic to the healthy targets in another Availability Zone\.

After you disable an Availability Zone, the targets in that Availability Zone remain registered with the load balancer\. However, even though they remain registered, the load balancer does not route traffic to them\.

### Cross\-zone load balancing<a name="cross-zone-load-balancing"></a>

The nodes for your load balancer distribute requests from clients to registered targets\. When cross\-zone load balancing is enabled, each load balancer node distributes traffic across the registered targets in all enabled Availability Zones\. When cross\-zone load balancing is disabled, each load balancer node distributes traffic only across the registered targets in its Availability Zone\.

The following diagrams demonstrate the effect of cross\-zone load balancing\. There are two enabled Availability Zones, with two targets in Availability Zone A and eight targets in Availability Zone B\. Clients send requests, and Amazon Route 53 responds to each request with the IP address of one of the load balancer nodes\. This distributes traffic such that each load balancer node receives 50% of the traffic from the clients\. Each load balancer node distributes its share of the traffic across the registered targets in its scope\.

If cross\-zone load balancing is enabled, each of the 10 targets receives 10% of the traffic\. This is because each load balancer node can route its 50% of the client traffic to all 10 targets\.

![\[When cross-zone load balancing is enabled\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/images/cross_zone_load_balancing_enabled.png)

If cross\-zone load balancing is disabled:
+ Each of the two targets in Availability Zone A receives 25% of the traffic\.
+ Each of the eight targets in Availability Zone B receives 6\.25% of the traffic\.

This is because each load balancer node can route its 50% of the client traffic only to targets in its Availability Zone\.

![\[When cross-zone load balancing is disabled\]](http://docs.aws.amazon.com/elasticloadbalancing/latest/userguide/images/cross_zone_load_balancing_disabled.png)

With Application Load Balancers, cross\-zone load balancing is always enabled\.

With Network Load Balancers, cross\-zone load balancing is disabled by default\. After you create a Network Load Balancer, you can enable or disable cross\-zone load balancing at any time\. For more information, see [Cross\-zone load balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/network-load-balancers.html#cross-zone-load-balancing) in the *User Guide for Network Load Balancers*\.

When you create a Classic Load Balancer, the default for cross\-zone load balancing depends on how you create the load balancer\. With the API or CLI, cross\-zone load balancing is disabled by default\. With the AWS Management Console, the option to enable cross\-zone load balancing is selected by default\. After you create a Classic Load Balancer, you can enable or disable cross\-zone load balancing at any time\. For more information, see [Enable cross\-zone load balancing](https://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-disable-crosszone-lb.html#enable-cross-zone) in the *User Guide for Classic Load Balancers*\.

## Request routing<a name="request-routing"></a>

Before a client sends a request to your load balancer, it resolves the load balancer's domain name using a Domain Name System \(DNS\) server\. The DNS entry is controlled by Amazon, because your load balancers are in the `amazonaws.com` domain\. The Amazon DNS servers return one or more IP addresses to the client\. These are the IP addresses of the load balancer nodes for your load balancer\. With Network Load Balancers, Elastic Load Balancing creates a network interface for each Availability Zone that you enable\. Each load balancer node in the Availability Zone uses this network interface to get a static IP address\. You can optionally associate one Elastic IP address with each network interface when you create the load balancer\.

As traffic to your application changes over time, Elastic Load Balancing scales your load balancer and updates the DNS entry\. The DNS entry also specifies the time\-to\-live \(TTL\) of 60 seconds\. This helps ensure that the IP addresses can be remapped quickly in response to changing traffic\.

The client determines which IP address to use to send requests to the load balancer\. The load balancer node that receives the request selects a healthy registered target and sends the request to the target using its private IP address\.

### Routing algorithm<a name="routing-algorithm"></a>

With **Application Load Balancers**, the load balancer node that receives the request uses the following process:

1. Evaluates the listener rules in priority order to determine which rule to apply\.

1. Selects a target from the target group for the rule action, using the routing algorithm configured for the target group\. The default routing algorithm is round robin\. Routing is performed independently for each target group, even when a target is registered with multiple target groups\.

With **Network Load Balancers**, the load balancer node that receives the connection uses the following process:

1. Selects a target from the target group for the default rule using a flow hash algorithm\. It bases the algorithm on: 
   + The protocol
   + The source IP address and source port
   + The destination IP address and destination port
   + The TCP sequence number

1. Routes each individual TCP connection to a single target for the life of the connection\. The TCP connections from a client have different source ports and sequence numbers, and can be routed to different targets\.

With **Classic Load Balancers**, the load balancer node that receives the request selects a registered instance as follows:
+ Uses the round robin routing algorithm for TCP listeners
+ Uses the least outstanding requests routing algorithm for HTTP and HTTPS listeners

### HTTP connections<a name="http-connections"></a>

Classic Load Balancers use pre\-open connections, but Application Load Balancers do not\. Both Classic Load Balancers and Application Load Balancers use connection multiplexing\. This means that requests from multiple clients on multiple front\-end connections can be routed to a given target through a single backend connection\. Connection multiplexing improves latency and reduces the load on your applications\. To prevent connection multiplexing, disable HTTP `keep-alives` by setting the `Connection: close` header in your HTTP responses\.

Classic Load Balancers support the following protocols on front\-end connections \(client to load balancer\): HTTP/0\.9, HTTP/1\.0, and HTTP/1\.1\.

Application Load Balancers support the following protocols on front\-end connections: HTTP/0\.9, HTTP/1\.0, HTTP/1\.1, and HTTP/2\. You can use HTTP/2 only with HTTPS listeners, and can send up to 128 requests in parallel using one HTTP/2 connection\. Application Load Balancers also support connection upgrades from HTTP to WebSockets\.

Both Application Load Balancers and Classic Load Balancers use HTTP/1\.1 on backend connections \(load balancer to registered target\)\. `Keep-alive` is supported on backend connections by default\. For HTTP/1\.0 requests from clients that do not have a host header, the load balancer generates a host header for the HTTP/1\.1 requests sent on the backend connections\. For Application Load Balancer, the host header contains the DNS name of the load balancer\. For Classic Load Balancer, the host header contains the IP address of the load balancer node\.

Application Load Balancers and Classic Load Balancers support pipelined HTTP on front\-end connections\. They do not support pipelined HTTP on backend connections\.

### HTTP headers<a name="http-headers"></a>

Application Load Balancers and Classic Load Balancers add **X\-Forwarded\-For**, **X\-Forwarded\-Proto**, and **X\-Forwarded\-Port** headers to the request\.

For front\-end connections that use HTTP/2, the header names are in lowercase\. Before the request is sent to the target using HTTP/1\.1, the following header names are converted to mixed case: **X\-Forwarded\-For**, **X\-Forwarded\-Proto**, **X\-Forwarded\-Port**, **Host**, **X\-Amzn\-Trace\-Id**, **Upgrade**, and **Connection**\. All other header names are in lowercase\.

Application Load Balancers and Classic Load Balancers honor the connection header from the incoming client request after proxying the response back to the client\.

When Application Load Balancers and Classic Load Balancers receive an **Expect** header, they respond to the client immediately with an HTTP 100 Continue without testing the content length header, remove the **Expect** header, and then route the request\.

### HTTP header limits<a name="http-header-limits"></a>

The following size limits for Application Load Balancers are hard limits that cannot be changed\.

**HTTP/1\.x headers**
+ Request line: 16 K
+ Single header: 16 K
+ Whole header: 64 K

**HTTP/2 headers**
+ Request line: 8 K
+ Single header: 8 K
+ Whole header: 64 K

## Load balancer scheme<a name="load-balancer-scheme"></a>

When you create a load balancer, you must choose whether to make it an internal load balancer or an internet\-facing load balancer\. Note that when you create a Classic Load Balancer in EC2\-Classic, it must be an internet\-facing load balancer\.

The nodes of an internet\-facing load balancer have public IP addresses\. The DNS name of an internet\-facing load balancer is publicly resolvable to the public IP addresses of the nodes\. Therefore, internet\-facing load balancers can route requests from clients over the internet\.

The nodes of an internal load balancer have only private IP addresses\. The DNS name of an internal load balancer is publicly resolvable to the private IP addresses of the nodes\. Therefore, internal load balancers can only route requests from clients with access to the VPC for the load balancer\.

Both internet\-facing and internal load balancers route requests to your targets using private IP addresses\. Therefore, your targets do not need public IP addresses to receive requests from an internal or an internet\-facing load balancer\.

If your application has multiple tiers, you can design an architecture that uses both internal and internet\-facing load balancers\. For example, this is true if your application uses web servers that must be connected to the internet, and application servers that are only connected to the web servers\. Create an internet\-facing load balancer and register the web servers with it\. Create an internal load balancer and register the application servers with it\. The web servers receive requests from the internet\-facing load balancer and send requests for the application servers to the internal load balancer\. The application servers receive requests from the internal load balancer\.