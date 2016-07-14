#<h2>Scaling and Load Balancing</h2>

The ConnexCS platform is highly scalable in terms of Channels, CPS and in single / multiple zones.

We aim to run all servers at a maximum of 50% their capacity. This enables us to accept unforecast bursts comfortably.

Although we benchmark much higher we recommend no more than 1000 channels / 100cps per server. However, this is always subject to traffic profiles.

One extra amazing feature of ConnexCS is that no matter how many servers you have or how many zones you have, all your configuration is available through a single control panel and data does not change if you add / remove servers, it’s still just as easy to use.

<h2>What is a zone?</h2>

A zone is a geographical region consisting of 1 or more datacenters, current zones are (UK, USA East, USA West, Germany, Amsterdam, Singapore).

<h2>How can I scale in a Single Zone?</h2>

Stage 1

Single Server.

Stage 2

2 Servers: If you wish to scale beyond a server in a single zone the first recommended method is primary / secondary. This 2 server setup points all calls to a single server which when capacity is reached (on a call by call basis) calls are passed over to the secondary server.

Stage 2 (Alternative).

If you are using DNS to provide connectivity to your customers, you can setup round robin or, even better, SRV records with your DNS provider to distribute calls on the DNS level. This allows multiple servers to act as the primary server on a call by call basis.

Stage 3

If your capacity requirement exceeds beyond this you can have a load balancer deployed. This is a single server which has the sole purpose of disseminating calls to other servers equally. The advantage here over the DNS load balancing option is: you remain in stricter control of your calls, they will be distributed evenly and you don’t require your customers hardware/software to support SRV records. With a Load Balancer you can have as many servers behind it as you want, and as the load balancer does minimal work, you can exceed well beyond 5,000 cps before you have to worry about anything.

<h2>How can I scale in Multiple Zones?</h2>

This is where you need to take a look at your traffic sources and destinations. You are best setting up your servers in proximity to your customers and/or providers. This allows the least latent route to be taken and ensures best delivery and quality of your service.

You can failover in multiple zones using the same techniques for a single zone, but always be aware of a latency hit when moving between different zones.

<h2>Best practices</h2>

Regardless of using load balancers, primary-secondary or single servers, correct DNS setup is always a good idea. DNS offers a level of redundancy that is completely independent of any underlying servers.

It’s fairly trivial to setup sip.uk.yourcompany.com and sip.usa.yourcompany.com (as an example). Then if there is any time of server downtime, capacity limitation, IP address changes you have full control to direct your traffic to a different location without interrupting your customers.

One stage even better if you are using multiple zones is, a Global Traffic Redirector on your domain. This means that you can give your customers a single sip.yourdomain.com and based on their IP proximity you can redirect them on a DNS level to the nearest zone. You can still then set-up various DNS failover if zones become unavailable.

<h2>Does using ConnexCS guarantee a HA (High Availability) solution?</h2>

This is very dependant on your specific setup. If you have a single server with us, the stability of that server will be as good as we can make it. However, a single server can still have faults and data center outages are still something that can happen which are completely beyond our control. But if you deploy across multiple zones you can achieve high availability.

<h2>How quickly could I deploy 5 servers in each zone with load balancers and DNS?</h2>

At present we can assist you in deploying a top end multi-zone geographically redundant carrier grade solution in less than 24 hours. This will be high throughput and tolerant of failures in multiple zones.

<h2>Can you help me with my DNS requirements?</h2>

Yes, we can advise you on these setting or we can manage the DNS service for you completely.