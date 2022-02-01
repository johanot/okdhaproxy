
Configuration of the HAProxy ingress controller to use ModSecurity as Web Application Firewall.

Official guide here: https://haproxy-ingress.github.io/docs/examples/modsecurity/

Firewall rules can then be applied to mitigate Log4Shell (cve-2021-44228) in accordance with:
https://coreruleset.org/20211213/crs-and-log4j-log4shell-cve-2021-44228/


Requires:

- Deployment of modsecurity spoa agent: ./modsecurity-agent.yaml (includes log4shell config)

- L4-director in cluster router config for modsecurity endpoints.

- Config of the ingress itself to use WAF interceptor for requests: https://haproxy-ingress.github.io/docs/examples/modsecurity/#configuring-haproxy-ingress

- Ingresses must be configured to actually use WAF:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  annotations:
    haproxy-ingress.github.io/waf: "modsecurity"
    haproxy-ingress.github.io/waf-mode: "deny"
```

It would be preferable to configure the above policies in the global controller config and possibly use admission control to prevent per-route overrides, depending on security policies.
