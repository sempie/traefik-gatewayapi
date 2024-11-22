# Traefik Gateway API Examples

### Cluster (If required)
```shell
k3d cluster create gatewayapi --port 80:80@loadbalancer --port 443:443@loadbalancer --port 9000:9000@loadbalancer --port 9443:9443@loadbalancer --k3s-arg "--disable=traefik@server:0"
```

### Gateway-API Install
```shell
kubectl apply -f https://github.com/kubernetes-sigs/gateway-api/releases/download/v1.2.0/experimental-install.yaml
```

### Cert-Manager
```shell
helm repo add jetstack https://charts.jetstack.io --force-update
helm upgrade --install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace \
  --set "extraArgs={--enable-gateway-api}" \
  --set crds.enabled=true
```

### Traefik & App Install
```shell
kubectl create namespace traefik
helm install traefik traefik/traefik \
  --values values.yaml \
  --namespace traefik
kubectl apply -f dashboard.yaml
kubectl apply -f traefik-gateway.yaml
kubectl apply -f routes.yaml
kubectl apply -f echo.yaml
kubectl apply -f issuer.yaml
kubectl apply -f certificate.yaml
kubectl apply -f https://raw.githubusercontent.com/traefik/traefik/v3.2/docs/content/reference/dynamic-configuration/kubernetes-whoami-svc.yml
```