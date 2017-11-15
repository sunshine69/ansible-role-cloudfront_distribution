# cloudfront_distribution

Creates a cloudfront distribution

## Variables

Note that the below variables can use `cloudfront_get_head` which is defined
to be a list containing `GET` and `HEAD`

* `cloudfront_cache_behaviors` - list of cache behaviors for various paths

  ```
  cloudfront_cache_behaviors:
    - path_pattern: /errors/*
      target_origin_id: "{{ env }}.{{ tld_name_external }}"
      allowed_methods:
        items: "{{ cloudfront_get_head }}"
        cached_methods: "{{ cloudfront_get_head }}"
      viewer_protocol_policy: redirect-to-https
      forwarded_values:
        query_string: no
  ```

* `cloudfront_certificate` - dict describing cloudfront certificate to use. Example:

  ```
  cloudfront_certificate:
    # certificate_arn gets set in the cloudfront_distribution role from
    # cloudfront_certificate_name
    acm_certificate_arn: "{{ certificate_arn }}"
    ssl_support_method: sni-only
    minimum_protocol_version: TLSv1
    certificate_source: acm
  ```

* `cloudfront_certificate_name` - name of the certificate to use for cloudfront
* `cloudfront_errors` - dictionary containing `codes` to handle,
  `pages` dict of default error page and override for specific codes, and `ttl`
  of error. Example:

  ```
    codes:
    - 500
    - 501
    - 502
    - 503
    - 504
  pages:
    default: /errors/5xx.html
    503: /errors/503.html
  ttl: 300
  ```

* `cloudfront_default_cache_behavior` - default cache behavior for cloudfront. Example:

  ```
  cloudfront_default_cache_behavior:
    viewer_protocol_policy: redirect-to-https
    target_origin_id: "{{ env }}.{{ tld_name_external }}"
    compress: no
    forwarded_values:
      headers:
        - Host
      cookies:
        forward: all
    allowed_methods:
      items:
        - GET
        - HEAD
        - DELETE
        - POST
        - PUT
        - OPTIONS
        - PATCH
      cached_methods:
        - GET
        - HEAD
  ```

* `cloudfront_ipv6_enabled` - whether or not IPv6 is enabled
* `cloudfront_logging` -  bucket to store access logs

  ```
  cloudfront_logging:
    enabled: yes
    include_cookies: no
    bucket: "{{ logging_bucket_name }}.s3.amazonaws.com"
    prefix: "cloudfront/{{ env }}"
  ```

* `cloudfront_origins` - list of origins to serve

  ```
  cloudfront_origins:
  - domain_name: "{{ env }}.{{ tld_name_external }}"
    id: "{{ env }}.{{ tld_name_external }}"
    custom_origin_config:
      origin_protocol_policy: https-only
  ```

* `cloudfront_waf` - name of Web Application Firewall to use (defaults to no WAF)
