"""Suppose client has a new requirement of setup service in one network and setup its management in another network. 
Client needs a setup of nginx as a web page hosting, but he has an issue as he needs a basic cache system + who can handle the requests smoothly + 
manages the requests according + need a middleware layer which helps them easily debug the service issue. So he called DevOps."""

"""You understood the problem statement and told them you would create 2 networks for management and service. 
For the middleware problem statement we need nginx again but as a reverse proxy."""

Kindly do the following day wise setup:

Day1: 
Create 2 vpc networks with min requirements
1 public subnet (management vpc and service vpc)
1 private subnet (management vpc)
2 private subnet (service vpc)
1 database private subnet (service vpc)
1 middleware private subnet (service vpc)

Create in such a way that both vpc networks can communicate with each other. 
Then setup nginx in service vpc under private subnet and use another nginx as a reverse proxy which can forward traffic from load balancer to nginx web hosting.

"""Client has another concern about security of the network, as not all resources should be allowed to
each resource, so kindly restrict the rest of the traffic in route tables and Nacl".

Day2:
You need to manage 
route tables of both the networks in such a way that only your teammates ip addresses should be allowed to igw rest should be deny.
Also manage Nacl group in such a way that it should allow traffic on specific port ranges to subnetworks.

"""While you are doing work on instance, some how your key 🗝️ is lost somewhere or currupted, client 
raising this issue as p0 and ask us for recover the key or kindly setup new key so that they can login the server atleast."""

Day3:
Kindly find the possible ways through which we can login to the server , you can do this 
Either by volume mounting
Either by doing configuration changes through user data.
Or by creating your own server key as temporary key to login there.

Kindly perform all three one by one and analyse the result.

NOTE: 
All traffic should be blocked only specific ports and IP addresses should be allowed.
No secret and access keys, only roles and policy
No root login. 

GOOD TO DO: 
If above part is done then only perform this scenario. CloudFront can cache objects and serve them directly to users (viewers), reducing the load on your Application Load Balancer. CloudFront can also help to reduce latency and even absorb some distributed denial of service (DDoS) attacks. So if you need these benefits with less load on LB and basic ddos protection kindly attach cloudfront network in front of load balancer on http traffic forwarding.
