# Rewrite Headers

Inspired by: https://github.com/XciD/traefik-plugin-rewrite-headers and https://github.com/Connection-Loops/traefik-plugin-rewrite-headers

Rewrite headers is a middleware plugin for [Traefik](https://traefik.io) which replace a header in the request and/or response

The only additional feature it has is, it supports manipulating Host header too. Which is not possible with original repos.

## Configuration

### Static

```yaml

experimental:
  plugins:
    rewriteHeaders:
      moduleName: github.com/Connection-Loops/traefik-plugin-rewrite-headers"
```

### Dynamic

To configure the Rewrite Request Header plugin you should create a [middleware](https://docs.traefik.io/middlewares/overview/) in your dynamic configuration as explained [here](https://docs.traefik.io/middlewares/overview/).
The following example creates and uses the rewriteHeaders middleware plugin to modify the Location header

```yaml
http:
  routes:
    my-router:
      rule: "Host(`localhost`)"
      service: "my-service"
      middlewares : 
        - "rewriteHeaders"
  services:
    my-service:
      loadBalancer:
        servers:
          - url: "http://127.0.0.1"
  middlewares:
    rewriteHeaders:
      plugin:
        rewriteHeaders:
          rewrites:
            request:
              - header: Location
                regex: "^http://(.+)$"
                replacement: "https://$1"
            response:
              - header: Location
                regex: "^http://(.+)$"
                replacement: "https://$1"
```

Label based configuration

``` yaml
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.request[0].header = Location
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.request[0].regex = ^http://(.+)$
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.request[0].replacement = https://$1
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.response[0].header = Location
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.response[0].regex = ^http://(.+)$
- traefik.http.middlewares.rewriteHeaders.plugin.rewriteHeaders.rewrites.response[0].replacement = https://$1
```
