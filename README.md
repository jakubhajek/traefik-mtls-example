##  mTLS example

This is a basic example presenting how to create the environment with two Traefik instances. The first instance that is called outside use has TLSPassthrough enabled and passes all HTTP requests to the another Traefik instance called inside. 

A simple diagram: 
```
Incoming request with client certificate -> Traefik outside (443/TCP) — (internal network) > Traefik Inside > An example service with mTLS
```

- Traefik Outside that is exposed to the Internet
- Traefik Inside that is running in local network
- Both are sharing the same Docker network

Prerequisites:

TLS passthrough from Outside to Inside. 
TlS terminated on the second layer. 
mTLS enabled on second layer on whoami application

### Generating certificates

```
minica -domains whoami.127.0.0.1.nip.io
```

## Validating the client authentication

```sh
curl --cert inside/certs/whoami.127.0.0.1.nip.io/cert.pem \
 --key inside/certs/whoami.127.0.0.1.nip.io/key.pem \
 https://whoami.127.0.0.1.nip.io -k
```

## Alternative implementation scenarios

There are also other use cases of using mTlS. Here are other examples that might be implemented:

- terminating mTLS on the 1st layer (outside) and then implement mTLS between outside and inside 
- mTLS on outside to inside instance and mTLS to the container 