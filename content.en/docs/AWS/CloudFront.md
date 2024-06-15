---
type: 'docs'
bookCollapseSection: false
bookHidden: false
bookToC: true
bookComments: false
bookSearchExclude: false
bookFlatSection: true
weight: 1
---

# What is CloudFront?

Amazon CloudFront is a web service that speeds up distribution of our static, dynamic web content, such as .html, .css, .js and image files to our end users.

When a end user requests content that we are serving with CloudFront, the request is routed to the edge location that provides the lowest latency(time delay), so that content is delivered with the best possible performance.

* If the content is already in the edge locatin with the lowest latency, CloudFront delivers it immediately.
* If the content is not in that edge location, CloudFront retrieves it from origin such as S3, HTTP Server, etc.

# How CloudFront works?

1. When a end user requests content, DNS routes the request to the POP that can best serve the request. In the POP, CloudFront checks its cache for the requested object. If the object is in the cache, CloudFront returns it.

2. If the object is not in the cache, the POP typically goes to the nearest regional edge cache to fetch it. 

3. If the object is not in the regional edge cache, CloudFront goes to the origin server to fetch the object.

![](/images/CloudFront.png)

# Pricing

CloudFront charges for data transfers out from its edge locations, along with HTTP or HTTPS requests. Pricing varies by usage type, geographic region, and feature selection.

The data transfer from our origin to CloudFront is free when using AWS origins like Amazon S3, ELB, API Gateway. We are only billed for the outbound data transfer from CloudFront to our end users when using AWS origins.

See [CloudFront pricing](https://aws.amazon.com/cloudfront/pricing/) for more details.

# References

* https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/Introduction.html
