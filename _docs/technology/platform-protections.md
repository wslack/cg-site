---
parent: technology
layout: docs
sidenav: true
title: Protections against malicious activity
weight: 30
---

Applications and services on the internet experience frequent attacks, probes, and other malicious traffic. Threat actors making malicious requests may aim to exploit vulnerabilities, to compromise infrastructure, or to deny service to legitimate clients of your applications.

As a multi-tenant platform, cloud.gov observes a diverse and frequent number of attacks. We are constantly improving our defenses to keep your applications online and unaffected by high-traffic-volume attacks against the platform and our customers. To achieve this, cloud.gov includes multiple layers of defense against different types of attacks.

## Blocking known malicious patterns

All inbound traffic to the cloud.gov platform are protected by a set of [web application firewall (WAF) rules](https://aws.amazon.com/waf/), which can block any traffic that matches known malicious patterns. The WAF rules for cloud.gov currently include managed rule sets offered by AWS that offer protection against:

- [Cross-site scripting (XSS)](https://owasp.org/www-community/attacks/xss/)
- Requests for invalid paths or extensions
- Requests from known or suspected malicious IP ranges
- Known Java exploits

The WAF rules for cloud.gov also include a custom rule set that blocks:

- Traffic attempting to exploit [Log4j vulnerabilities](https://www.cisa.gov/news-events/news/apache-log4j-vulnerability-guidance)
- Traffic from [fuzzing](https://owasp.org/www-community/Fuzzing) or probing tools that have been identified from logs
- Traffic matching [path traversal attack patterns](https://owasp.org/www-community/attacks/Path_Traversal)

## Protections against traffic surges

Occasionally, cloud.gov sees significant spikes in traffic that may be attempting to overwhelm platform infrastructure via a [DDoS attack](https://www.cloudflare.com/learning/ddos/what-is-a-ddos-attack/) or may simply be a very large scale probing attack.

In order to mitigate the effect of traffic surges on the platform, cloud.gov includes the following rate limits for requests:

- Traffic coming through CloudFront is rate limited with a [CHALLENGE action][challenge action] to **2000 requests** per **forwarded IP address** per 5 minutes
- Traffic not coming through CloudFront is rate limited with a [CHALLENGE action][challenge action] to **2000 requests** per **source IP address** per 5 minutes

### AWS CloudFront & CDNs

Another protection against traffic surges available on the platform is the ability to use [Amazon CloudFront as a CDN for your application]({{ site.baseurl }}{% link _docs/services/external-domain-service.md %}). Among its other benefits, CloudFront can cache requests based on configurable patterns. Since cached requests will be handled by CloudFront and not reach your application, they offer some protection against floods of traffic.

## Reporting impact on legitimate traffic

If you suspect that your traffic is being improperly affected by these protections, please contact us at [support@cloud.gov]({{ site.support_email }}).

---

### Page information

- Last modified on: {% last_modified_at %}

[challenge action]: https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge.html
