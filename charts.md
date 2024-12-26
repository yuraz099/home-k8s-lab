helm repo add twingate https://twingate.github.io/helm-charts
helm upgrade --install twingate-stalwart-rooster twingate/connector -n default --set connector.network="yuriydzyuba2" --set connector.accessToken="" --set connector.refreshToken=""
helm upgrade --install twingate-congenial-dugong twingate/connector -n default --set connector.network="yuriydzyuba2" --set connector.accessToken="" --set connector.refreshToken=""

helm repo add jetstack https://charts.jetstack.io
helm repo update
kubectl apply -f https://github.com/cert-manager/cert-manager/releases/download/v1.6.3/cert-manager.crds.yaml

helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --version v1.6.3 \
  # --set installCRDs=true

openssl req -x509 -newkey rsa:4096 -nodes -out tls.crt -keyout tls.key -days 365
kubectl create secret tls chart-tls --key tls.key --cert tls.crt



reset microk8s certs for logs

sudo microk8s refresh-certs -e ca.crt
microk8s config > ~/.kube/config