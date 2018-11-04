---
title: CNAME at apex - a website publisher perspective
abbrev: CNAME at apex - a website publisher perspective
docname: draft-york-dnsop-cname-at-apex-publisher-view-00
category: info

ipr: trust200902
area: General
workgroup: DNSOP Working Group
keyword: Internet-Draft

stand_alone: yes
pi: [toc, sortrefs, symrefs]

author:
 -
    ins: D. York
    name: Dan York
    organization: Internet Society
    email: york@isoc.org

normative:
  RFC2119:

informative:


--- abstract

There has been a large amount of discussion about the "CNAME at apex" issue
within the DNSOP Working Group. This draft provides the perspective of one 
publisher of multiple websites about why CNAME-like functionality is desirable
at the apex of a domain zone.

--- middle

# Introduction

From the early days of the Web, publishers of websites generally operated their
main public website using the "www" subdomain, as in "www.example.com".

In recent years many organizations have moved to dropping the "www" and referring
to their main website by simply the domain name, as in "example.com". There are numerous
reasons for this change, including the simplicity of saying or writing the name and also smaller address bars on mobile browsers. If you are designing a large advertisement to display in, say, an airport hallway, you can make the domain name address larger and more visibile if you simply use "example.com" and drop the "www". Additionally, some web browsers are no longer showing the full URL (or even any of the URL) and so the "www" is no longer visible.

Regardless of the reasons, the fact is that many website publishers and marketing/communications teams are moving to using only the domain name without the "www" or other subdomains to reference their public website. 

The expectation is that: 1) users will be able to simply enter "example.com" into their
browser; and 2) users will only see "example.com" in their address bar (if URLs are even displayed).

# The Challenge

If the organization's website is a simple web server with specific A and AAAA records, 
this change can be easily implemented by adding the appropriate A and AAAA records at
the zone apex.

However, most larger site publishers (and many smaller publishers) now use content distribution networks (CDNs), global load balancers, or some other form of a caching / load balancing network in front of their website. The result is that there is no single "A" or "AAAA" record for the organization's website.

Publishers use CDNs for a variety of reasons, including:

* Performance - connecting users to an edge server with the lowest latency
* Geo-location - connecting users to edge servers appropriate for their geography
* DDoS / security - using various security mechanisms provided by CDNs
* IPv6 - using a CDN to offer IPv6 access when the origin server is only on IPv4
* TLS - using a CDN to offer higher levels of TLS usage than possible with origin server

While there may be many non-CDN mechanisms to address those reasons, website publishers in 2018 are increasingly using CDNs as a simple business solution.

The DNS issue is that the website publisher is told to simply redirect all traffic to some address within the CDN along the lines of:

    a123qkt5y7xxb3df8.example-cdn.net

The CDN then performs its service of providing the requesting client with the A or AAAA record most appropriate for the client's network/geographic location.

NOTE: Some CDNs require that they manage the DNS services for the target domain name. The publisher must designate the CDN's DNS servers as the authoritative name servers (NS records) for the domain. At that point the CDN handles all of this directly. However, this document is discussing CDNs where the publisher retains control of serving out DNS records for the domain.

# CNAME works for subdomains

For websites using a subdomain such as "www", this is simply done in DNS using CNAME:

    www.example.com  300  IN  CNAME a123qkt5y7xxb3df8.example-cdn.net

Now all web traffic to www.example.com is redirected to the CDN address. The CDN returns the appropriate records. All is fine.

# CNAME at apex does not work
    
For reasons outlined in Appendix C of {{?I-D.ietf-dnsop-aname}} CNAME does not work at the apex of a domain. A primary reason is section 3.6.2 of {{?RFC1034}}.

# Propriety solutions

To respond to this business demand, various DNS operators (including CDN providers who also act as DNS operators) have developed proprietary solutions (also referred to as "stupid DNS tricks" within the DNSOP community). Under various names such as "URL flattening" or "CNAME flattening", these techniques do work and allow the sites to be accessible on the CDN via the simple domain name.

However, because of the proprietary nature, they then lock the website publisher into using that DNS operator / CDN.  The website publisher does not have an easily ability to move to a different DNS operator / CDN unless the new DNS operator / CDN can also provide a mechanism to allow the non-www domain usage.

Additionally, many large web site publishers use (or want to use) multiple CDNs to achieve various business objectives, including resiliency / availability. The lack of a standard mechanism to do this "CNAME at apex" functionality limits the ability to explore multi-CDN options.

# Existing and proposed solutions

Various email discussions have indicated that existing mechanisms such as SRV and URI records can address this issue, although deployment/adoption concerns have been raised. Multiple solutions have been proposed for discussion at IETF 103, including:

* {{?I-D.ietf-dnsop-aname}}
* {{?I-D.bellis-dnsop-http-record}}

As a website publisher, I have a business objective to meet - make the site accessible over "example.com" versus "www.example.com". As long as this can happen in a manner that can be widely deployed, I am not partial to any specific solution. I just want it to work and not lock me in to specific proprietary solutions.

# Past discussion

There was a lengthy discussion of this topic in the DNSOP session at IETF 102. Slides from that session are useful for more context:

* https://datatracker.ietf.org/meeting/102/materials/slides-102-dnsop-somethingapex-02
* https://datatracker.ietf.org/meeting/102/materials/slides-102-dnsop-cnameapex-00

# Conventions and Definitions

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED", "NOT RECOMMENDED", "MAY", and "OPTIONAL" in this
document are to be interpreted as described in BCP 14 {{RFC2119}} {{!RFC8174}}
when, and only when, they appear in all capitals, as shown here.

# Security Considerations

TODO add any security considerations.

# IANA Considerations

This document has no IANA actions.



--- back

# Acknowledgments
{:numbered="false"}

TODO acknowledge.
