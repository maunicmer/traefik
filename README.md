Create and chmod 600 acme.json

# Route example

data/traefik/dynamic.yml 

http:
  routers:
    host:
      rule: "Host(`host.domain.com`)"
      entryPoints:
        - "websecure"
      service: "host"
      tls:
        certResolver: letsencrypt-legacy
        options:
  services:
    host:
     loadBalancer:
       servers:
         - url: 'http://172.16.1.2:80'



data/traefik/config.yml

http:
  middlewares:
    secureHeaders:
      headers:
        sslRedirect: true
        forceSTSHeader: true
        stsIncludeSubdomains: true
        stsPreload: true
        stsSeconds: 31536000
    user-auth:
     basicAuth:
       users:
         - "user:$apr1$ed21l4ja$ekMNLLccccccccKk/eKxxxxxxKR0"
  routers:
    traefik:
      rule: "Host(`traefik.domain.com`)"
      service: "api@internal"
      middlewares:
        - "user-auth"
tls:
  options:
    default:
      cipherSuites:
        - TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
        - TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
        - TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
        - TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
        - TLS_ECDHE_ECDSA_WITH_CHACHA20_POLY1305
        - TLS_ECDHE_RSA_WITH_CHACHA20_POLY1305
      minVersion: VersionTLS12
