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

Management-VPC

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/27e4b10c-c99b-4115-a473-178fc70c2066)

Service-VPC

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/104647d5-4f39-4119-8129-d63accb0ad13)


Create in such a way that both vpc networks can communicate with each other. 

#### VPC peering:-

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/7aa81138-8d98-43cc-a077-fb28aa7826bd)

#### Output:

Service ---> Management

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/7bfca72b-a688-483f-995c-27e4d032461a)

Management ---> Service

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/59237c97-3c5b-4a3e-b88a-6ea628da8950)


Then setup nginx in service vpc under private subnet and use another nginx as a reverse proxy which can forward traffic from load balancer to nginx web hosting.

    server {
        listen 80;
        server_name example.com;
    
        location / {
            proxy_pass http://backend-server-1;
        }
    
        location /app2/ {
            proxy_pass http://backend-server-2;
        }
    }

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/51e1bd5e-889d-4142-be3c-d6d326e28321)


![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/0121d30d-79d0-4671-93a0-a2266d740d23)

#### Error

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/89468b1c-9e88-4811-b950-9319f27bc1f4)

#### Middleware nginx config: 

root@ip-10-0-140-62:/etc/nginx/sites-enabled# cat default 

    server {
    listen 80;
    server_name localhost;
    location / {
        proxy_pass http://10.0.28.86;
    }
    }

#### Output:

target grp --> middleware instance

![image](https://github.com/HarshitSingh-Codes/aws-practice/assets/67234531/42abb13b-ac9f-40ab-bdbb-1a5e7544b111)







"""Client has another concern about security of the network, as not all resources should be allowed to

each resource, so kindly restrict the rest of the traffic in route tables and Nacl".

Day2:

You need to manage route tables of both the networks in such a way that only your teammates ip addresses should be allowed to igw rest should be deny.

Also manage Nacl group in such a way that it should allow traffic on specific port ranges to subnetworks.
    
    nmcli -p device show


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
