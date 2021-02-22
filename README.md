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
