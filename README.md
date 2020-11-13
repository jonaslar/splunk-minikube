# splunk-minikube

## Add ingress

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

## Change host file

/etc/hosts:
192.168.99.111 splunk.example.com

## Splunk Operator

https://splunk.github.io/splunk-operator/#creating-splunk-enterprise-deployments

