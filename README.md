# splunk-minikube

## Install Splunk Operator

Info, https://splunk.github.io/splunk-operator/#creating-splunk-enterprise-deployments 

> kubectl apply -f https://github.com/splunk/splunk-operator/releases/download/0.2.0/splunk-operator-install.yaml

## Add Splunk Custom Resource for StandAlone server

``` s1
cat <<EOF | kubectl apply -f -
apiVersion: enterprise.splunk.com/v1beta1
kind: Standalone
metadata:
  name: s1
  finalizers:
  - enterprise.splunk.com/delete-pvc
EOF
```

## Add ingress

``` ingress
kind: Ingress
apiVersion: extensions/v1beta1
metadata:
  name: splunk-ingress
  namespace: default
  annotations:
    kubernetes.io/ingress.class: nginx
    nginx.ingress.kubernetes.io/affinity: cookie
spec:
  rules:
    - host: splunk.example.com
      http:
        paths:
          - path: /
            pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8000
          - path: /services/collector
            pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8088
    - host: deployer.splunk.example.com
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8000
    - host: cluster-master.splunk.example.com
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8000
    - host: license-master.splunk.example.com
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8000
    - host: spark-master.splunk.example.com
      http:
        paths:
          - pathType: ImplementationSpecific
            backend:
              serviceName: splunk-s1-standalone-headless
              servicePort: 8009
```

## Change host file

/etc/hosts:
192.168.99.111 splunk.example.com


## Run SCFK Chard

 helm install my-splunk-connect -f values.yaml https://github.com/splunk/splunk-connect-for-kubernetes/releases/download/1.4.3/splunk-connect-for-kubernetes-1.4.3.tgz
