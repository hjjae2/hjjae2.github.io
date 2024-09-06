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

# Data key caching

* https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/data-key-caching.html
* https://docs.aws.amazon.com/encryption-sdk/latest/developer-guide/thresholds.html

Data key caching is an optional feature of the AWS Encryption SDK that **you should use cautiously**. By default, the AWS Encryption SDK generates a 
**new data key for every encryption operation**. This technique supports cryptographic best practices, which discourage excessive reuse of data keys. ⭐

<br>

In general, use data key caching only when it is required to meet **your performance goals.** Then, use the data key caching security thresholds to 
ensure that **you use the minimum amount of caching** required to meet your cost and performance goals.

## Security thresholds
**The security thresholds help you to limit how long each cached data key is used and how much data is protected under each data key.**
* Maximum age (required)
* Maximum messages encrypted (optional)
* Maximum bytes encrypted (optional)
