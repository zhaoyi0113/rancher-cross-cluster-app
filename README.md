
- Package helm

```
cd cqrs
helm repo index . --url zhaoyi0113.github.io/rancher-cross-cluster-app/cqrs/cqrs-0.1.0.tgz
helm package .
```

- Deploy

1. Deploy secret, `kubectl apply -f extra/secret.yaml`

2. Deploy cloud secret, `kubectl apply -f extra/awsSecret.yaml`, `kubectl apply -f extra/gcpSecret.yaml`

3. Deploy helm package

4. Deploy ALB

- Configure kubectl for GCP: `gcloud container clusters get-credentials gcp-cqrs --zone australia-southeast2-a`

- Configure kubectl for AWS: `aws eks update-kubeconfig --name aws-cqrs`

- Client

```
curl -XPOST k8s-sidecar-3eabe19e18-1188815636.ap-southeast-2.elb.amazonaws.com/transaction -d '{"amount": 1}'
```
