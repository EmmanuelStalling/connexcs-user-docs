# Table of Contents

- [Table of Contents](#table-of-contents)
- [Scaling and Load Balancing](#scaling-and-load-balancing)
    - [Capacity Failover](#capacity-failover)
    - [DNS Load Balancing](#dns-load-balancing)
        -[SRV Records](#srv-records)
    - [Best practices](#best-practices)
    - [Clusters](#clusters)
    - [What is a zone?](#what-is-a-zone)
    - [How can I scale in a Single Zone?](#how-can-i-scale-in-a-single-zone)
    - [How can I scale in Multiple Zones?](#how-can-i-scale-in-multiple-zones)
    - [FAQ - Scaling and Load Balancing](#faq---scaling-and-load-balancing)


# Scaling and Load Balancing

The ConnexCS platform is highly scalable in terms of Channels, CPS, and single or multiple zones.  All configurations are available through a single control panel, no matter how many servers or zones are present.  Our goal is to run all servers at a maximum of 50% capacity, which allows us to handle unforeseen bursts of traffic effectively. A single server can scale vertically upwards of 5,000 channels and 500 cps (calls per second). While we benchmark for these limits, we recommend no more than 1,000 channels / 100 cps per server to avoid downtimes. This is always subject to traffic profiles, however.

## Capacity Failover

If you have multiple servers in your network, **Capacity Failover** helps eliminate overload on any single server. If a server reaches capacity (CPS or Channels), it responds with a 302 instead of a 500,  directing another server in its cluster to handle the call. Capacity failover is not the same as load-balancing, though, nor is it mutually exclusive. It is a mechantic to allow for some overflow from any specific zones without drops.  

## DNS Load Balancing

Load balancing through DNS is a good way to instill scalability in your system, where a managed DNS offers control over the dissemination of traffic. In fact, it is highly recommended that providers route calls via DNS records rather than IP addresses even in single-server setups. If you need to make any changes apart from firewall rules, you don’t need to contact customers and the operation can continue without pause.

**DNS Management**

We offer fully managed X.sip.direct domain name that can be used to map servers. Our Anycast DNS service allows the fastest response times with the lowest TTL (60 seconds). For a sip.mydomain.com pointing to your server, set a long TTL CNAME (1 hour+) to our X.sip.direct name for your server. It ensures you'll have the best possible vanity address independant from our system, while still benefitting from our DNS management.

Single servers with my-server.sip.direct can just use a standard A record, where changing the server IP requires updating the record in the control panel. Additional servers are added to the same A record, and the DNS server will round-robin  the responses, splitting traffic evenly among the servers. However, a high throughput client will cause traffic to bounce between the servers every sixty(60) seconds.

### SRV Records
SRV records are an improvement on the DNS level for SIP, as they are widely supported and allow you to move the load-balancing instructions from hidden behind the DNS servers to inside the SIP UA. This means that with an SRV record, you could nitially split the traffic to servers A and B, then failover to server C if A and B fail.

## Best practices

Proper DNS setup is always a good idea, as DNS offers a level of redundancy completely independent of its underlying servers. If you are using multiple zones, use a Global Traffic Redirector on your domain.  You can give your customers a single SIP and, based on their IP proximity, redirect them to the nearest zone using DNS. 

One reason providers don't use SRV records alone is they come with more reliance on customers and carrier UA to perform the load balancing, and changes are delayed. Load balancers are an upgrade to this solution, as they are are designed to handle 1000+ cps easily, and to provide more advanced routing capabilities, especially with registrations.  

Load balancers still send calls between multiple zones, and can grant additional boosts in availabilty.  Be adivised: a load bearer creates a single point of failure that can negate the security of redundancies. For this, we have two solutions:

* Deploy two load balancers (ideally in different zones) then setup your SRV record to either load balance or primary/secondary.
* Set your SRV record to primary point to the load balancer and secondary send to the servers behind the load balancer. This way your load balancer will take responsibility untill it is unable to, then your network will fall back to the individual servers load balanced by your DNS SRV records.

## Clusters

Clusters are a new function that allow you to group one or more servers into a cluster in the same zone or between zones, as well as shared channel and CPS capacity allocation. If you use multiple servers and have capacity dynamically allocated between them, a cluster provides the best way to manage them. For example, imagine servers A, B, and C exist in different zones. If server A reaches capacity and traffic is redirected to server B, the cluster checks between all nodes to establish server capacity in real-time, and decides on a call-by-call basis whether to throttle capacities across the whole platform. 

Clusters also provider faster user location lookups. This may go unnoticed on smaller deployments, but since nodes are aware of registration states of every other server, it can make resolving the correct registered endpoints in larger deployments dramatically faster.

## What is a zone?

A zone is a geographical region consisting of one or more datacenters. Current zones are: UK, USA East, USA West, Germany, Amsterdam, and Singapore. Scaling across zones is the same as scaling within a zone, the difference being they are located in different geographical regions. Generally, this strategy for increasing capacity, but it can also be used to improve availability.

## How can I scale in a Single Zone?
The following lists the methods for scaling in a single zone:

**Stage 1**

Single Server.

**Stage 2**

Two(2) Servers: If you need to scale beyond a server in a single zone, the recommended method is primary / secondary. This two-server setup points all calls to a single server which that passes calls secondary server when capacity is reached. Statistically, the possibility of two servers failing is lower, so the additional cost of more servers should be weighed against the value of additional failsafes for high-volume systems.

**Stage 2 (Alternative)**

If you use  DNS to provide connectivity, you can set up round robin or SRV records with the provider to distribute calls at the DNS level. This allows multiple servers to act as the primary server on a call-by-call basis.

**Stage 3**

If your capacity requirements exceed the above, can deploy a load balancer, a server dedicated to spreading calls to other servers equally. The advantage over DNS is that you maintain stricter control of your calls, assuring they are distributed evenly without using your customers' resources to support SRV records. With a Load Balancer, you can exceed well beyond 5,000 CPS before any concerns arise.

## How can I scale in Multiple Zones?

This is where you need to look at traffic sources and destinations. Setting up servers in proximity to customers and providers allows the least latent routes, ensuring a more consistent quality of service.  You can also failover in multiple zones using the same techniques for a single zone, but remember, a latency hit occurs when you move between different zones.

## FAQ  -  Scaling and Load Balancing

**Does using ConnexCS guarantee a High Availability (HA) solution?**

This depends on the specifics of your setup. If you have a single server, the stability of that server will be as good as we can make it. However, a single server can still have faults and data center outages that are beyond our control. If you deploy across multiple zones, however, you can achieve high availability.

**How quickly could I deploy Five(5) servers in each zone with load balancers and DNS?**

Currently, we can assist in deploying a top end multi-zone geographically redundant carrier grade solution in less than 24 hours. This will be high throughput and tolerant of failures in multiple zones.

**Can you help me with my DNS requirements?**

Yes. We can advise on DNS settings, or we can manage the DNS service for you completely.
