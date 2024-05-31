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

# What is resolver?

Configuration directive 'resolver' is used to specify the IP address of a name server that will be
used to resolve domain names of upstream servers.

https://nginx.org/en/docs/http/ngx_http_core_module.html#resolver

# What is 'valid' parameter?

By default, nginx caches answers using the TTL value of a name server response. An optional valid
parameter allows overriding it.

```nginx configuration
resolver 8.8.8.8 valid=5s;
```

# What is the problem?

The 'valid' parameter is not working as expected. It is not overriding the TTL value of a name
server response.

The reason is when the hostname is **fixed**, the resolver will cache the IP address of the hostname
and ignore the 'valid' parameter.

```nginx configuration
resolver 8.8.8.8 valid=5s;

server {
    server_name example.com;
    
    location / {
        proxy_pass http://example.com;
    }
}
```

# How to solve it?

Use **variable** instead of the fixed hostname.

```nginx configuration
resolver 8.8.8.8 valid=5s;

server {
    server_name example.com;
    
    location / {
        set $backend "example.com";
        proxy_pass http://$backend;
    }
}
```

# References

* https://stackoverflow.com/a/56487286