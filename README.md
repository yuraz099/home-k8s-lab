home k8s server 

Network notes:
all the stuff above was designed to run locally and through vpn or some tunneling protocol.
to makes this fully work, execute the following:

```
cat <<EOF | kubectl apply -f -
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
  labels:
    addonmanager.kubernetes.io/mode: EnsureExists
    k8s-app: kube-dns
data:
  Corefile: |
    .:53 {
        errors
        health {
          lameduck 5s
        }
        ready
        log . {
          class error
        }
        kubernetes cluster.local in-addr.arpa ip6.arpa {
          pods insecure
          fallthrough in-addr.arpa ip6.arpa
        }
        rewrite name regex (.*)\.dio\.porco ingress-nginx-internal-controller.nginx-system.svc.cluster.local
        prometheus :9153
        forward . /etc/resolv.conf
        cache 30
        loop
        reload
        loadbalance
    }
EOF

kubectl rollout restart deployment/coredns -n kube-system
```

don't pay too much attention to the domain name, it's just a personal thing.

if you don't like it just ctrl-f ctrl-r on the whole project.


Volume notes:
volumes are configured to be mounted on hostpath of the host machine, 
in future the configuration should cover also nfs protocol.
