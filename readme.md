# Traefik Gateway API Examples

### Cluster
```shell
k3d cluster create gatewayapi --port 80:80@loadbalancer --port 443:443@loadbalancer --port 9000:9000@loadbalancer --port 9443:9443@loadbalancer --k3s-arg "--disable=traefik@server:0"
```

### Gateway-API Install
```shell
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.2.0/experimental-install.yaml
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.2/docs/content/reference/dynamic-configuration/kubernetes-gateway-rbac.yml
```

### Traefik CRD Install
```shell
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.2/docs/content/reference/dynamic-configuration/kubernetes-crd-definition-v1.yml
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.2/docs/content/reference/dynamic-configuration/kubernetes-crd-rbac.yml

```

### Self Signed Cert Install
```shell
openssl genrsa -out tls.key 2048
openssl req -new -key tls.key -out tls.csr -subj "/CN=example.com"
openssl x509 -req -days 365 -in tls.csr -signkey tls.key -out tls.crt
kubectl create secret tls mysecret --cert=tls.crt --key=tls.key
```

### Traefik & App Install
```shell
kubectl create namespace traefik
kubectl apply -f traefik.yaml
kubectl apply -f traefik-gateway.yaml
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.2/docs/content/reference/dynamic-configuration/kubernetes-whoami-svc.yml
```