## Public Hosted Zone

Two types of hosted zone
- Public Hosted Zone
- Private Hosted Zone

A hosted zone is a DNS database specifically for a domain such as arsamrahmaan.com

Route53 is global resilient service

Hosted zones are created automatically when you register a domain using Route53

You can also create a hosted zone separately if you want to register a domain elsewhere and use Route53 to host it.

There is a monthly fee to host each hosted zone and a fee for the queries made against that hosted zone

So when you register a domain, name servers for that domain are entered in to the top level domain zone. And these point at your name servers and then your name servers and the zone that they host become authoritative for that domain.

Now a public hosted zone is a DNS database,so a zone file,which is hosted by Route53,on a public name server. And this means it's accessible from public internet and within VPC using Route53 resolver.

Architecturally when you create a public hosted zone, Route53 allocate 4 public name servers, and it's on those nameservers that the zone file is hosted.And to integrate it with the pblic DNS system,you change the name server records for that domain to point at those 4 Route53 name servers.

Inside a public hosted zone, you create resource records, which are the actual item of data, which DNS uses.


## Private Hosted Zone

It is not public , as it is associated with VPCs within AWS and it's only accessible within VPCs that it's associated with.

you can associate private hosted zone with VPCs in your account using console UI, CLI and API and even in different accounts if you use API and CLI only.

Everything else is same. You can use them to create resource records, and these are resolvable within VPCs.

It is also possible to use a technique called split-view or split-horizon DNS,which is where you have public and private hosted zone of the same name. You might do this if you want certain system to be accessible via your bsiness DNS, but only within your environment, and the public website when anyone access from outside of your corporate network.


## CName vs Route53 Alias

The domain itself with no record is apex. 
so www.netflix.com isnt the apex, netflix.com is. 
www.cantrill.io isn't the apex, cantrill.io is. 
(netflix.com/cantrill.io)->its the naked domain, the domain on its own, the apex.) 

A cname is a record, which points at another name. 
so www.netflix.com => server1.netflix.com is an example. 

An A record is .. name => IP. 
so www.netflix.com => 1.3.3.7

An alias can be an A Alias, or a CNAME Alias,its just a special type of A or CNAME

Alias is free to use.

Alias can be used at the apex of the domain

Many AWS services like ELB, they do not give you an IP address to use,which they give you a DNS name.This means if you only use CNAME names pointing the naked netflix.com at an ELB, would not be supported.

See the ScreenShots for more.

There are many record like A record, AAA record, MX record, TXT record, NS record, A alias record, CName alias record.

## Simple Routing

It does **not support** health checks.
It supports one record per name.
Each record have multipple values.
All values are returned in a random order.
Client chooses and use one value.
Use simple routing when you want to route request towards **one service** such as web server

## Health Checks

Health checks are separate from Route53 but are used by records inside Route53. It means you do not create checks with in records but you configure them separately.

Health checks are performed by health checkers which are distributed globally.This means if you are checking the health of systems which are hosted on public internet, then you need to allow these checks to occur from the health checkers.

Healh checks are not just limited to aws targets,you can check anything which is accessible over the public internet. It just needs an IP address. The checks occur every 30 seconds by default or can be increased to every 10 seconds at an additional cost.

Checks can be TCP where route53 tries to establish a TCP connection and needs to be successful within 10 seconds.

With HTTP and HTTPS checks you can perform string matching, route53 tries to establish a TCP connection and needs to be successful within 4 seconds, and in addition, endpoint must respond with a HTTP status code in 200 range or 300 range with in 2 seconds after connection and route53 health checker when it receives the status code it must also receive the response body from the end point within the next 2 seconds. Route 53 searches for the string that you specify form the response body. The string must appear entirely in the first 5,120 bytes in the response body, otherwise the endpoint fails the health checks.This way you can check the app is responding correctly by checking the response and now based on these health checks,an endpoint can be checked wheather it's healthy or unhealthy.

Checks can be of three types:
- Endpoint : these are checks which access the health of an actual endpoint that you specify.
- CloudWatch Alarm Checks.
- Checks of checks : We can create health checks which will check applications wide health.

## MultiValue Routing

See ScreenShots.

## Weighted Routing

(see the ss first)

Health checks do not remove records from the calculation. It will just skip the unhealthy record and move to next until a healthy record is selected.
