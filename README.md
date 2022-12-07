- Deploy EKS ALB

1. Deploy EKS and nodegroup

2. Create a role for service account in AWS. The role needs to have policy have access to OIDC connector. Follow this link to create one: https://docs.aws.amazon.com/eks/latest/userguide/enable-iam-roles-for-service-accounts.html

3. Deploy service account for alb `kubectl apply -f extra/serviceAccountAlb.yaml`, replace with the role arn created above.

4. Deploy cert manager `kubectl apply --validate=false -f https://github.com/jetstack/cert-manager/releases/download/v1.5.4/cert-manager.yaml`

5. Deploy load balancer controller follow: https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html

6. 

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

url -XPOST http://rancher.crms.myzeller.dev/transaction -d '{"amount": 1}'
```
